# Brain

Category: REV

Points: xxx

Solves: xxx

>Help! A hacker said that this "language" has a flag but I can't find it!

### Solution

```
>+++++++++++[<++++++++++>-]<[-]>++++++++[<++++++>-]<[-]>++++++++[<++++++>-]<[-]>++++++++++++++[<+++++++>-]<[-]>+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++[<++>-]<[-]>+++++++++++++++++++++++++++++++++++++++++[<+++>-]<[-]>+++++++[<+++++++>-]<[-]>+++++++++++++++++++[<+++++>-]<[-]>+++++++++++[<+++++++++>-]<[-]>+++++++++++++[<++++>-]<[-]>+++++++++++[<++++++++++>-]<[-]>+++++++++++++++++++[<+++++>-]<[-]>+++++++++++[<+++++++++>-]<[-]>++++++++[<++++++>-]<[-]>++++++++++[<++++++++++>-]<[-]>+++++++++++++++++[<+++>-]<[-]>+++++++++++++++++++[<+++++>-]<[-]>+++++++[<+++++++>-]<[-]>+++++++++++[<++++++++++>-]<[-]>+++++++++++++++++++[<+++++>-]<[-]>++++++++++++++[<+++++++>-]<[-]>+++++++++++++++++++[<++++++>-]<[-]>+++++++++++++[<++++>-]<[-]>+++++++[<+++++++>-]<[-]>+++++++++++[<++++++++++>-]<[-]>+++++++++++++++++[<++++++>-]<[-]>+++++++[<++++++>-]<[-]>+++++++++++[<+++++++++>-]<[-]>+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++[<+>-]<[-]>+++++++++++[<+++>-]<[-]>+++++++++++++++++++++++++[<+++++>-]<[-]
```

Looks like some brainfuck code. Putting it into a [visualizer](https://ashupk.github.io/Brainfuck/brainfuck-visualizer-master/index.html), we can see that it creates a loop that increments the first index to `110` (`n`) then clears it to `0`, then increments to `48` (`0`), clears it, and so on. So it seems to load the flag char by char, and then deleting it. Upon further analysis, we can see that the loop that clears the value in the tape is `[-]`. Knowing this, we can [modify the code](https://gchq.github.io/CyberChef/#recipe=Find_/_Replace(%7B'option':'Simple%20string','string':'%5B-%5D'%7D,'.%3E',true,false,true,false)&input=PisrKysrKysrKysrWzwrKysrKysrKysrPi1dPFstXT4rKysrKysrK1s8KysrKysrPi1dPFstXT4rKysrKysrK1s8KysrKysrPi1dPFstXT4rKysrKysrKysrKysrK1s8KysrKysrKz4tXTxbLV0%2BKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrK1s8Kys%2BLV08Wy1dPisrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrWzwrKys%2BLV08Wy1dPisrKysrKytbPCsrKysrKys%2BLV08Wy1dPisrKysrKysrKysrKysrKysrKytbPCsrKysrPi1dPFstXT4rKysrKysrKysrK1s8KysrKysrKysrPi1dPFstXT4rKysrKysrKysrKysrWzwrKysrPi1dPFstXT4rKysrKysrKysrK1s8KysrKysrKysrKz4tXTxbLV0%2BKysrKysrKysrKysrKysrKysrK1s8KysrKys%2BLV08Wy1dPisrKysrKysrKysrWzwrKysrKysrKys%2BLV08Wy1dPisrKysrKysrWzwrKysrKys%2BLV08Wy1dPisrKysrKysrKytbPCsrKysrKysrKys%2BLV08Wy1dPisrKysrKysrKysrKysrKysrWzwrKys%2BLV08Wy1dPisrKysrKysrKysrKysrKysrKytbPCsrKysrPi1dPFstXT4rKysrKysrWzwrKysrKysrPi1dPFstXT4rKysrKysrKysrK1s8KysrKysrKysrKz4tXTxbLV0%2BKysrKysrKysrKysrKysrKysrK1s8KysrKys%2BLV08Wy1dPisrKysrKysrKysrKysrWzwrKysrKysrPi1dPFstXT4rKysrKysrKysrKysrKysrKysrWzwrKysrKys%2BLV08Wy1dPisrKysrKysrKysrKytbPCsrKys%2BLV08Wy1dPisrKysrKytbPCsrKysrKys%2BLV08Wy1dPisrKysrKysrKysrWzwrKysrKysrKysrPi1dPFstXT4rKysrKysrKysrKysrKysrK1s8KysrKysrPi1dPFstXT4rKysrKysrWzwrKysrKys%2BLV08Wy1dPisrKysrKysrKysrWzwrKysrKysrKys%2BLV08Wy1dPisrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrWzwrPi1dPFstXT4rKysrKysrKysrK1s8KysrPi1dPFstXT4rKysrKysrKysrKysrKysrKysrKysrKysrWzwrKysrKz4tXTxbLV0) to replace `[-]` with `.>` which sends the current char to stdout, and moves us to the next index of the tape.

Running this new code we get:

![Decode](/images/Brain.png)

### Flag

```n00bz{1_c4n_c0d3_1n_br41nf*ck!}```