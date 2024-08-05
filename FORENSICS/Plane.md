# Plane

Category: FORENSICS

Points: 100

Solves: xxx

>So many plane-related challenges! Why not another one? The flag is the latitude, longitude of the place this picture is taken from, rounded upto two decimal places.

### Solution

<img src="/images/plane.jpg" height="400"> (Image for the challenge)

First scan image for metadata:

```
$ exiftool -n plane.jpg
...
GPS Latitude                    : 13.37
GPS Longitude                   : -13.37
GPS Position                    : 13.37 -13.37
```

Prob fake coords but `13.37,-13.37`!



### Flag

```n00bz{13.37,-13.37}```