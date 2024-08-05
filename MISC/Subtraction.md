# Subtraction

Category: MISC

Points: xxx

Solves: xxx

>My little brother is learning math, can you show him how to do some subtraction problems?

### Solution

```py
import random

n = 696969

a = []
for i in range(n):
    a.append(random.randint(0, n))
    a[i] -= a[i] % 2

print(' '.join(list(map(str, a))))

for turns in range(20):
    c = int(input())
    for i in range(n):
        a[i] = abs(c - a[i])

    if len(set(a)) == 1:
        print(open('/flag.txt', 'r').read())
        break
```

Another classic puzzle, we are given `696969` randomly generated even integers and we must turn them all into the same value after `20` moves. In each move, we can provide an integer `c`, then every value will be subtracted from `c` and absolute valued. The optimal move ordering is to send:

```
524288
262144
131072
65536
32768
16384
8192
4096
2048
1024
512
256
128
64
32
16
8
4
2
1
```

Where each next `c` is half of the previous. The reason why this works is, at first $0 \leq a_i \leq 696969 $, subtracting $a_i$ from $524288$ means $-524288 \leq a_i \leq -172681$, absolute value $a_i$ leads to $0 \leq 172681 \leq a_i \leq 524288$. Now $a_i$ is bounded by $524288$. If we do this again. $0 \leq a_i \leq 524288$, subtracting $a_i$ from $262144$ yields $-262144 \leq a_i \leq 262144$, absolute value $a_i$ leads to $0 \leq a_i \leq 262144$. Through this process we can keep on reduce the bounding by half. $0 \leq a_i \leq 262144$, $0 \leq a_i \leq 131072$, $0 \leq a_i \leq 65536$, ... , $0 \leq a_i \leq 1$. Finally, we reduce $a_i$ down to 0 or 1, and since all $a_i$ were even to begin with, every $a_i$ will be 1 (since we start even, and subtract even numbers except for the last 1). Now that we have all $a_i = 1$, we get the flag!

```py
from pwn import *

p = remote("challs.n00bzunit3d.xyz", 10290)
cur = 2**19 # 524288
for i in range(20):
    p.sendline(str(cur).encode('ascii'))
    cur//=2

p.interactive()
# output
# ... 198980 475128 584442 556982 520212 85584 130974 18028
# n00bz{1_sh0uld_t34ch_my_br0th3r_logs_48f45b4728df}
```

### Flag

```n00bz{1_sh0uld_t34ch_my_br0th3r_logs_48f45b4728df}```