# FlagChecker

Category: REV

Points: 431

Solves: xxx

>Why did the macros hide its knowledge? Because it didn't want anyone to "excel"!

### Solution

We are giving a Excel Spreadsheets file and we can use tools like `olevba` to extract the VBA macros to get:

```vb
Sub FlagChecker()
    Dim chars(1 To 24) As String
    guess = InputBox("Enter the flag:")
    If Len(guess) <> 24 Then
        MsgBox "Nope"
    End If
    char_1 = Mid(guess, 1, 1)
    char_2 = Mid(guess, 2, 1)
    char_3 = Mid(guess, 3, 1)
    char_4 = Mid(guess, 4, 1)
    char_5 = Mid(guess, 5, 1)
    char_6 = Mid(guess, 6, 1)
    char_7 = Mid(guess, 7, 1)
    char_8 = Mid(guess, 8, 1)
    char_9 = Mid(guess, 9, 1)
    char_10 = Mid(guess, 10, 1)
    char_11 = Mid(guess, 11, 1)
    char_12 = Mid(guess, 12, 1)
    char_13 = Mid(guess, 13, 1)
    char_14 = Mid(guess, 14, 1)
    char_15 = Mid(guess, 15, 1)
    char_16 = Mid(guess, 16, 1)
    char_17 = Mid(guess, 17, 1)
    char_18 = Mid(guess, 18, 1)
    char_19 = Mid(guess, 19, 1)
    char_20 = Mid(guess, 20, 1)
    char_21 = Mid(guess, 21, 1)
    char_22 = Mid(guess, 22, 1)
    char_23 = Mid(guess, 23, 1)
    char_24 = Mid(guess, 24, 1)
    If (Asc(char_1) Xor Asc(char_8)) = 22 Then
        If (Asc(char_10) + Asc(char_24)) = 176 Then
            If (Asc(char_9) - Asc(char_22)) = -9 Then
                If (Asc(char_22) Xor Asc(char_6)) = 23 Then
                    If ((Asc(char_12) / 5) ^ (Asc(char_3) / 12)) = 130321 Then
                        If (char_22 = char_11) Then
                            If (Asc(char_15) * Asc(char_8)) = 14040 Then
                                If (Asc(char_12) Xor (Asc(char_17) - 5)) = 5 Then
                                    If (Asc(char_18) = Asc(char_23)) Then
                                        If (Asc(char_13) Xor Asc(char_14) Xor Asc(char_2)) = 121 Then
                                            If (Asc(char_14) Xor Asc(char_24)) = 77 Then
                                                If 1365 = (Asc(char_22) Xor 1337) Then
                                                    If (Asc(char_10) = Asc(char_7)) Then
                                                        If (Asc(char_23) + Asc(char_8)) = 235 Then
                                                            If Asc(char_16) = (Asc(char_17) + 19) Then
                                                                If (Asc(char_19)) = 107 Then
                                                                    If (Asc(char_20) + 501) = (Asc(char_1) * 5) Then
                                                                        If (Asc(char_21) = Asc(char_22)) Then
                                                                            MsgBox "you got the flag!"
                                                                        End If
                                                                    End If
                                                                End If
                                                            End If
                                                        End If
                                                    End If
                                                End If
                                            End If
                                        End If
                                    End If
                                End If
                            End If
                        End If
                    End If
                End If
            End If
        End If
    End If
End Sub
```

Essentially, the flag is 24 chars long and is broken into `char1` through `char24`. These chars then must follow a set of conditions, so let's [clean](https://gchq.github.io/CyberChef/#recipe=Find_/_Replace(%7B'option':'Regex','string':'%20%20'%7D,'',true,false,true,false)Find_/_Replace(%7B'option':'Regex','string':'If%20'%7D,'',true,false,true,false)Find_/_Replace(%7B'option':'Regex','string':'%20Then'%7D,'',true,false,true,false)&input=SWYgKEFzYyhjaGFyXzEpIFhvciBBc2MoY2hhcl84KSkgPSAyMiBUaGVuCiAgICAgICAgSWYgKEFzYyhjaGFyXzEwKSArIEFzYyhjaGFyXzI0KSkgPSAxNzYgVGhlbgogICAgICAgICAgICBJZiAoQXNjKGNoYXJfOSkgLSBBc2MoY2hhcl8yMikpID0gLTkgVGhlbgogICAgICAgICAgICAgICAgSWYgKEFzYyhjaGFyXzIyKSBYb3IgQXNjKGNoYXJfNikpID0gMjMgVGhlbgogICAgICAgICAgICAgICAgICAgIElmICgoQXNjKGNoYXJfMTIpIC8gNSkgXiAoQXNjKGNoYXJfMykgLyAxMikpID0gMTMwMzIxIFRoZW4KICAgICAgICAgICAgICAgICAgICAgICAgSWYgKGNoYXJfMjIgPSBjaGFyXzExKSBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICBJZiAoQXNjKGNoYXJfMTUpICogQXNjKGNoYXJfOCkpID0gMTQwNDAgVGhlbgogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIElmIChBc2MoY2hhcl8xMikgWG9yIChBc2MoY2hhcl8xNykgLSA1KSkgPSA1IFRoZW4KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgSWYgKEFzYyhjaGFyXzE4KSA9IEFzYyhjaGFyXzIzKSkgVGhlbgogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgSWYgKEFzYyhjaGFyXzEzKSBYb3IgQXNjKGNoYXJfMTQpIFhvciBBc2MoY2hhcl8yKSkgPSAxMjEgVGhlbgogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIElmIChBc2MoY2hhcl8xNCkgWG9yIEFzYyhjaGFyXzI0KSkgPSA3NyBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIElmIDEzNjUgPSAoQXNjKGNoYXJfMjIpIFhvciAxMzM3KSBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBJZiAoQXNjKGNoYXJfMTApID0gQXNjKGNoYXJfNykpIFRoZW4KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBJZiAoQXNjKGNoYXJfMjMpICsgQXNjKGNoYXJfOCkpID0gMjM1IFRoZW4KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgSWYgQXNjKGNoYXJfMTYpID0gKEFzYyhjaGFyXzE3KSArIDE5KSBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBJZiAoQXNjKGNoYXJfMTkpKSA9IDEwNyBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgSWYgKEFzYyhjaGFyXzIwKSArIDUwMSkgPSAoQXNjKGNoYXJfMSkgKiA1KSBUaGVuCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIElmIChBc2MoY2hhcl8yMSkgPSBBc2MoY2hhcl8yMikpIFRoZW4K) and analyze those next:

```
(Asc(char_1) Xor Asc(char_8)) = 22
(Asc(char_10) + Asc(char_24)) = 176
(Asc(char_9) - Asc(char_22)) = -9
(Asc(char_22) Xor Asc(char_6)) = 23
((Asc(char_12) / 5) ^ (Asc(char_3) / 12)) = 130321
(char_22 = char_11)
(Asc(char_15) * Asc(char_8)) = 14040
(Asc(char_12) Xor (Asc(char_17) - 5)) = 5
(Asc(char_18) = Asc(char_23))
(Asc(char_13) Xor Asc(char_14) Xor Asc(char_2)) = 121
(Asc(char_14) Xor Asc(char_24)) = 77
1365 = (Asc(char_22) Xor 1337)
(Asc(char_10) = Asc(char_7))
(Asc(char_23) + Asc(char_8)) = 235
Asc(char_16) = (Asc(char_17) + 19)
(Asc(char_19)) = 107
(Asc(char_20) + 501) = (Asc(char_1) * 5)
(Asc(char_21) = Asc(char_22))
```

From here you can probably use some solver, but manually decoding it works as well:

```
known
 1 = n
 2 = 0
 3 = 0
 4 = b
 5 = z
 6 = {

24 = }
```

- `(Asc(char_19)) = 107` : `char_19 = k`
- `(Asc(char_20) + 501) = (Asc(char_1) * 5)` : `char_20 = 5*n-501 = 1`
- `(Asc(char_1) Xor Asc(char_8)) = 22` : `char_8 = n^22 = x` 
    - `(Asc(char_15) * Asc(char_8)) = 14040` : `char_15 = 14040/x = u`
    - `(Asc(char_23) + Asc(char_8)) = 235` : `char_23 = 235-x = s`
        - `(Asc(char_18) = Asc(char_23))` : `char_18 = s`
- `((Asc(char_12) / 5) ^ (Asc(char_3) / 12)) = 130321` : `char_12 = 130321**(1/('0'/12)) * 5 = 130321^0.25*5 = 19*5 = _`
    - `(Asc(char_12) Xor (Asc(char_17) - 5)) = 5` : `char_17 = 5^_+5 = _`
        - `Asc(char_16) = (Asc(char_17) + 19)` : `char_16 = _+19 = r`
- `(Asc(char_22) Xor Asc(char_6)) = 23` : `char_22 = {^23 = l`
    - `(Asc(char_21) = Asc(char_22))`: `char_21 = l`
    - `(char_22 = char_11)`: `char_11 = l`
    - `(Asc(char_9) - Asc(char_22)) = -9` : `char_9 = l-9 = c`
- `(Asc(char_10) + Asc(char_24)) = 176` : `char_10 = 176-} = 3`
    - `(Asc(char_10) = Asc(char_7))` : `char_7 = 3`
- `(Asc(char_14) Xor Asc(char_24)) = 77` : `char_14 = 77^} = 0`
    - `(Asc(char_13) Xor Asc(char_14) Xor Asc(char_2)) = 121` : `char_13 = 121^'0'^'0' = y` 
    
```
 1 = n
 2 = 0
 3 = 0
 4 = b
 5 = z
 6 = {
 7 = 3
 8 = x
 9 = c
10 = 3
11 = l
12 = _
13 = y
14 = 0
15 = u
16 = r
17 = _
18 = s
19 = k
20 = 1
21 = l
22 = l
23 = s
24 = }
```

DONE!

### Flag

```n00bz{3xc3l_y0ur_sk1lls}```