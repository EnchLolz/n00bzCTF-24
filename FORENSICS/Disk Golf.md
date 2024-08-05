# Disk Golf

Category: FORENSICS

Points: xxx

Solves: xxx

>Let's play some disk golf!

### Solution

Looks like we are given a disk image to analyze. We can use tools like sleuthkit to look through what's inside. We first use `fls` to list all files and directories inside (grep out `flag.txt` after looking through it once), then we can use `icat` to print the data inside a node:

![Solve](/images/Disk.png)

These numbers look like octal, so we can quickly convert them back to chars:

```py
>>> ''.join(chr(int(x,8)) for x in "156 60 60 142 172 173 67 150 63 137 154 60 156 147 137 64 167 64 61 164 63 144 137 144 61 65 153 137 146 60 162 63 156 163 61 143 65 175".split())
'n00bz{7h3_l0ng_4w41t3d_d15k_f0r3ns1c5}'
```

### Flag

```n00bz{7h3_l0ng_4w41t3d_d15k_f0r3ns1c5}```