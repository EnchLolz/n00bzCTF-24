# Wave

Category: FORENSICS

Points: xxx

Solves: xxx

>The Wave is not audible, perhaps corrupted? 

### Solution

https://static.n00bzunit3d.xyz/Forensics/Wave/chall.wav has broken audio file so use curl to download the file:

```bash
curl https://static.n00bzunit3d.xyz/Forensics/Wave/chall.wav -o wave.wav
```

Open it up to see why it's corrupted:

![Bytes of File](/images/WaveZeros.png)

It seems like the magic bytes and other import data encoding information in the header has been replaces by `0`s. Doing a bit of [research](https://en.wikipedia.org/wiki/WAV#WAV_file_header), we can see what we are supposed to put:

![Research](/images/WaveResearch.png)

It looks like the `0`s have replace the `FileTypeBlockID` = `RIFF`, `FileFormatID` = `WAVE`, `FormatBlockID` = `fmt `, and `DataBlocID` = `data`:

![Replaced Wav](/images/WaveReplace.png)

Playing the audiofile now, not only does it work, but we also hear morse code!

![Wav Morse](/images/WaveMorse.png)

Decoding we get `beepbopmorsecode`!



### Flag

```n00bz{beepbopmorsecode}```