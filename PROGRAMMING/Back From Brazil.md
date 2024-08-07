# Back From Brazil

Category: PROGRAMMING

Points: 465

Solves: 149

>I might have dropped a couple of eggs on my way to Brazil. Help me find the most optimal path back home.

### Solution

```py
import random, time

def solve(eggs):
    redactedscript = """
    â–ˆ â–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
    â–ˆâ–ˆ â–ˆ â–ˆâ–ˆ

    â–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
        â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆ

    â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ

    â–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
        â–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
            â–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆ
                â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ

            â–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆ
                â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
            â–ˆâ–ˆ â–ˆ â–ˆâ–ˆ â–ˆâ–ˆ
                â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆ â–ˆâ–ˆâ–ˆ

            â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ

    â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ
    """

    return sum([ord(c) for c in redactedscript])

n = 1000

start = time.time()

for _ in range(10):
    eggs = []
    for i in range(n):
        row = []
        for j in range(n):
            row.append(random.randint(0, 696969))
            print(row[j], end=' ')
        eggs.append(row)
        print()

    solution = solve(eggs)
    print("optimal: " + str(solution) + " ðŸ¥š")
    inputPath = input()
    inputAns = eggs[0][0]
    x = 0
    y = 0

    for direction in inputPath:
        match direction:
            case 'd':
                x += 1
            case 'r':
                y += 1
            case _:
                print("ðŸ¤”")
                exit()

        if x == n or y == n:
            print("out of bounds")
            exit()

        inputAns += eggs[x][y]



    if inputAns < solution:
        print(inputAns)
        print("you didn't find enough ðŸ¥š")
        exit()
    elif len(inputPath) < 2 * n - 2:
        print("noooooooooooooooo, I'm still in Brazil")
        exit()

    if int(time.time()) - start > 60:
        print("you ran out of time")
        exit()

print("tnxs for finding all my ðŸ¥š")
f = open("/flag.txt", "r")
print(f.read())
```

Essentially, we are given a 1000x1000 grid of random numbers, we start at cell `0,0` and need to end on `999,999`. We are only allowed to move right and down, and the sum of the numbers on our path has to be the max. We can do a simple DP solution, and backtrack our path:

```py
from pwn import *


n=1000

p = remote("24.199.110.35", 43298)

for i in range(10):
    # pad grid with 0's left and up
    grid = [[0]*(n+1)]

    for i in range(n):
        grid.append([0]+list(map(int,p.recvline().strip().decode().split())))

    path = [[0]*(n+1) for i in range(n+1) ]

    # DP
    for i in range(1,n+1):
        for j in range(1,n+1):
            if grid[i][j-1] > grid[i-1][j]:
                grid[i][j]+=grid[i][j-1]
                path[i][j] = "r"
            else:
                grid[i][j]+=grid[i-1][j]
                path[i][j] = "d"


    # Debug stuff
    print(opt:=p.recvline())
    opt = int(opt.decode().split()[1])
    print(f"{opt =}")
    print(f"{grid[-1][-1] =}")

    #recover path
    sol = ""
    cur = [n,n]
    while cur != [1,1]:
        direc = path[cur[0]][cur[1]]
        sol = direc+sol
        if direc == 'd':
            cur[0]-=1
        else:
            cur[1]-=1

    print(sol)
    p.sendline(sol.encode("ascii"))

p.interactive()
# output
# tnxs for finding all my 🥚
# n00bz{1_g0t_b4ck_h0m3!!!}
```

### Flag

```n00bz{1_g0t_b4ck_h0m3!!!}```