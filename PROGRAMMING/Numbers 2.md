# Numbers 2

Category: PROGRAMMING

Points: 403

Solves: xxx

>Let's see if you can do more than just counting... (Part 1 was in n00bzCTF 2023.)

### Solution

They tell us that there are only 3 questions, after a bit of testing we find they are:

1. Give me the greatest prime factor of `n`
2. Give me the least common multiple of `n1` and `n2`
3. Give me the greatest common divisor of `n1` and `n2`

These are very easy to code, python math module has `gcd` and `lcm`, sympy has `factorint` (I didn't know how big n could be, this turned out to be a bit overkill):

```py
from pwn import *
import math
from sympy import factorint

def largest_prime_factor(n):
    factors = factorint(n)
    return max(factors.keys())


p = remote("challs.n00bzunit3d.xyz", 10006)

p.recvline()


for i in range(100):
    print(p.recvline())
    print(q:=p.recvuntil(b":"))
    q = q.strip().strip(b":").decode().split()
    # hacky input handling
    n1 = q[-3]
    n2 = int(q[-1])
    if "prime" not in q:
        n1 = int(n1)
    # GCD
    if "divisor" in q:
        p.sendline(str(math.gcd(n1,n2)).encode('ascii'))
    # LCM
    elif "least" in q:
        p.sendline(str(math.lcm(n1,n2)).encode('ascii'))
    # Prime
    else:
        p.sendline(str(largest_prime_factor(n2)).encode('ascii'))
    p.recvline()

p.interactive()
```

```
...
b'Current round: 98 of 100\n'
b'Give me the greatest prime factor of 3278:'
b'Current round: 99 of 100\n'
b'Give me the greatest prime factor of 7401:'
b'Current round: 100 of 100\n'
b'Give me the greatest common divisor of 3477 and 5520:'
[*] Switching to interactive mode
Good job! Here's your flag: n00bz{numb3r5_4r3_fun_7f3d4a_32d7c972deb8}
```

### Flag

```n00bz{numb3r5_4r3_fun_7f3d4a_32d7c972deb8}```