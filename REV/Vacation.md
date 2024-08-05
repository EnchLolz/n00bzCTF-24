# Vacation

Category: REV

Points: 100

Solves: 590

>My friend told me they were going on vacation, but they sent me this weird PowerShell script instead of a postcard!

### Solution

```powershell
$bytes = [System.Text.Encoding]::ASCII.GetBytes((cat .\flag.txt))
[System.Collections.Generic.List[byte]]$newBytes = @()
$bytes.ForEach({
    $newBytes.Add($_ -bxor 3)
    })
$newString =  [System.Text.Encoding]::ASCII.GetString($newBytes)
echo $newString | Out-File -Encoding ascii .\output.txt
```

```
m33ayxeqln\sbqjp\twk\{lq~
```

The powershell script seems to just be xor cipher with value `3`:

![Vacation](/images/Vacation.png)


### Flag

```n00bz{from_paris_wth_xor}```