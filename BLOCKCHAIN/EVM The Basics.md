# EVM The Basics

Category: BLOCKCHAIN

Points: xxx

Solves: xxx

>I have some EVM runtime bytecode, whatever that means. You need to find the value, in hex, that you need to send to make the contract STOP and not self destruct. Wrap the hex in n00bz{}.

### Solution

We are given some bytecode `5f346113370265fdc29ff358a314601257ff00` and we can decompile it with https://app.dedaub.com/decompile giving us:

```
    0x0: PUSH0     
    0x1: CALLVALUE 
    0x2: PUSH2     0x1337
    0x5: MUL       
    0x6: PUSH6     0xfdc29ff358a3
    0xd: EQ        
    0xe: PUSH1     0x12
   0x10: JUMPI     
   0x11: SELFDESTRUCT
   0x12: STOP      
```

We can go through this line by line to see what we are doing:
1. `PUSH0` does nothing
2. `CALLVALUE` is our input pushed onto the stack -> | input |
3. `PUSH2` pushes 0x1337 onto the stack -> | input | 0x1337 |
4. `MUL` multiplies top 2 items on stack -> | input*0x1337 |
5. `PUSH6` pushes 0xfdc29ff358a3 onto the stack -> | input*0x1337 | 0xfdc29ff358a3 |
6. `EQ` checks if top 2 values of stack are equal
7. `PUSH1` pushes 0x12 to be used in `JUMPI`
8. `JUMPI` jumps to line 0x12 if `EQ` was true
9. `SELFDESTRUCT` BAD \>\:(
10. `STOP` GOOD \:)

From this we can see that all we need to find is some `x` such that `x*0x1337 = 0xfdc29ff358a3`. After some quick maths we get that `x = 56721355765` or `0xd34db33f5`

### Flag

```n00bz{0xd34db33f5}```