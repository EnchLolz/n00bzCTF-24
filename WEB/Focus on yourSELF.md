# Focus on yourSELF

Category: WEB

Points: 429

Solves: xxx

>Have you focused on yourself recently?

### Solution

From the docker-compose, we know that the flag is stored in an ENV variable. Now let's play around with the website, there is not much to the main page besides a button to view and upload:

![Home](/images/SelfHome.png)

Clicking on view gives us an image of `nature.jpg`

![View](/images/SelfView.png)

Clicking on upload, we can upload any file we want and we will view it back with a random id string:

![Upload](/images/SelfUpload.png)
![Uploaded](/images/SelfUploaded.png)

After going through a ton of rabbit holes :sob: (like I was trying to find exploits in frp that they used to host the thing and uploading adobe stock photos of people with filename self), I realized that there was a very simple LFI attack. When we put in the parameter `?image=nature.jpg` it was opening the file `nature.jpg`, so what if we try to open `etc/passwd`:

![attempt 1](/images/Selfetcpasswd1.png)
![attempt 2](/images/Selfetcpasswd2.png)

Boom, we have accessed `etc/passwd` (decoding the base64 gives us the contents). Cool, now that we have LFI, we can get the flag through `/proc/self/environ` which stores all environment variables (the SELF reference :0). Decoding the base64 gets us the flag!

![Environ File](/images/Selfproc.png)
![Decode](/images/SelfB64Decode.png)




### Flag

```n00bz{Th3_3nv1r0nm3nt_det3rmine5_4h3_S3lF_1e1a432c8964}```