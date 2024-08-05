# Sillygoose

Category: PROGRAMMING

Points: 267

Solves: xxx

>There's no way you can guess my favorite number, you silly goose.

### Solution

```py
from random import randint
import time
ans = randint(0, pow(10, 100))
start_time = int(time.time())
turns = 0
while True:
    turns += 1

    inp = input()

    if int(time.time()) > start_time + 60:
       print("you ran out of time you silly goose") 
       break

    if "q" in inp:
        print("you are no fun you silly goose")
        break

    if not inp.isdigit():
        print("give me a number you silly goose")
        continue

    inp = int(inp)
    if inp > ans:
        print("your answer is too large you silly goose")
    elif inp < ans:
        print("your answer is too small you silly goose")
    else:
        print("congratulations you silly goose")
        f = open("/flag.txt", "r")
        print(f.read())

    if turns > 500:
        print("you have a skill issue you silly goose")
```

>"Learn Binary Search" - ezraft

```py
from pwn import *

p = remote("24.199.110.35", 41199)

l,r = 0, pow(10, 100)

while l < r:
    mid = (l+r)//2
    p.sendline(str(mid).encode("ascii"))
    ans = p.recvline()
    if b'small' in ans:
        l = mid+1
    elif b'large' in ans:
        r = mid+1
    else:
        print(ans)
        print(p.recvline())
        p.interactive
        break
# output
# b'congratulations you silly goose\n'
# b'n00bz{y0u_4r3_4_sm4rt_51l1y_g0053}\n'
```


### Flag

```n00bz{y0u_4r3_4_sm4rt_51l1y_g0053}```