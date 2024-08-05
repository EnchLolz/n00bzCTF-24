# The Gang 3

Category: OSINT

Points: 486

Solves: 95

>Can you find out where the OG meetup point is? The flag is in the format n00bz{lat,long} with upto 3 decimal places, rounded. Continue where you left off... Note: Wikipedia can be wrong sometimes \;)

### Solution

Continue where we left off (Again). We see another twitter post, this time with an AES encrypted text, and tells us the key is the same as last time with IV being lat,long of a city met up in 2022:

![Twitter Post](/images/TheGang3TwitterPost.png)

From our previous exploration, we know that n00bzCTF 2022 and 2023 had this John Doe character, so we can look [there](https://github.com/n00bzUnit3d) first:

![n00bz 2022](/images/TheGang3n00bz2022.png)

![n00bz 2023](/images/TheGang3n00bz2023.png)


Great we found the coords and the key, now we just plug into [CyberChef](https://gchq.github.io/CyberChef/#recipe=AES_Decrypt(%7B'option':'UTF8','string':'YouCanNeverCatchJohnDoe!'%7D,%7B'option':'UTF8','string':'46.720,33.154'%7D,'GCM','Hex','Raw',%7B'option':'Hex','string':'d5e749da6b02c75cb4c763939632503a'%7D,%7B'option':'Hex','string':''%7D)&input=MTc2Mjg0MWQxODg4ZjZiMDI1ODE5OTBhYmRmMGFhYmEzNzVjODVmZDM4MTFhNmZiNDA1Nzc1ZmI4ZA) to get:

![Decrypt](/images/TheGang3AES.png)

A discord link??? Ok, we go into the Discord server and scroll past the convos about bedwars to see a conversation about flights in India and the OG meetup point:

![Discord Messages 1](/images/TheGang3Msgs1.png)

![Discord Messages 2](/images/TheGang3Msgs2.png)

With this information, we can just google for Indian statues and scroll through wikipedia pages (https://en.wikipedia.org/wiki/List_of_the_tallest_statues_in_India). After a bit of time we narrow it down to https://en.wikipedia.org/wiki/Statue_of_Prosperity as it's located on an airport!

Going to [Google maps](https://www.google.com/maps/place/Nadaprabhu+Kempegowda+Statue/@13.199147,77.6820516,148m/data=!3m1!1e3!4m10!1m2!2m1!1sstatue+of+prosperity!3m6!1s0x3bae1dd817d14fd1:0x623a16dea1b6fc4c!8m2!3d13.1991681!4d77.6822418!15sChRzdGF0dWUgb2YgcHJvc3Blcml0eZIBEnRvdXJpc3RfYXR0cmFjdGlvbuABAA!16s%2Fg%2F11q4mbltqy?entry=ttu) we can easily locate it:

![Maps](/images/TheGang3Maps.png)

coords at `13.199,77.682`

### Flag

```n00bz{13.199,77.682}```
