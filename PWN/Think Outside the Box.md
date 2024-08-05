# Think Out Side the Box

Category: PWN

Points: 464

Solves: 152

>You cannot beat my Tic-Tac-Toe bot! If you do, you get a flag! 

### Solution

Just treat the code, as a blackbox, and use the hint of thinking outside the box. I assumed that they would prob not check for a negative index, and yup sending a -1, completely breaks the bot \:D


```bash
$ nc challs.n00bzunit3d.xyz 10585
Welcome to Tic-Tac-Toe! In order to get the flag, just win! The bot goes first!
 _ | _ | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Move: 0, -1
 _ | _ | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Bot turn!
 _ | _ | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Move: 0, 0
 O | _ | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Bot turn!
 O | _ | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Move: 0, 1
 O | O | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Bot turn!
 O | O | _ 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
Move: 0, 2
Returning!
 O | O | O 
---|---|---
 _ | X | _ 
---|---|---
 _ | _ | _ 
You won! Flag: n00bz{l173r4lly_0u7s1d3_7h3_b0x_L0L_b938b52444a6}
```

Of couse if we look at the code it makes sense:
Main:
![Main](/images/PwnMain.png)

Move (takes in `i,j` checks that both are < 3):
![Move](/images/PwnMove.png)

Bot (uses hardcoded conditions so moving outside the grid breaks it):
![Bot](/images/PwnBot.png)


### Flag

```n00bz{l173r4lly_0u7s1d3_7h3_b0x_L0L_b938b52444a6}```