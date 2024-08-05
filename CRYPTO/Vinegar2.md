# Vinegar 2

Category: CRYPTO

Points: 135

Solves: 479

>Never limit yourself to only alphabets! 

### Solution

```py
alphanumerical = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*(){}_?'
matrix = []
for i in alphanumerical:
	matrix.append([i])

idx=0
for i in alphanumerical:
	matrix[idx][0] = (alphanumerical[idx:len(alphanumerical)]+alphanumerical[0:idx])
	idx += 1

flag=open('../src/flag.txt').read().strip()
key='5up3r_s3cr3t_k3y_f0r_1337h4x0rs_r1gh7?'
assert len(key)==len(flag)
flag_arr = []
key_arr = []
enc_arr=[]
for y in flag:
	for i in range(len(alphanumerical)):
		if matrix[i][0][0]==y:
			flag_arr.append(i)

for y in key:
	for i in range(len(alphanumerical)):
		if matrix[i][0][0]==y:
			key_arr.append(i)

for i in range(len(flag)):
	enc_arr.append(matrix[flag_arr[i]][0][key_arr[i]])
encrypted=''.join(enc_arr)
f = open('enc.txt','w')
f.write(encrypted)
```

```
*fa4Q(}$ryHGswGPYhOC{C{1)&_vOpHpc2r0({
```

The python script just creates the vigenere table, used the encode the message. What we really care about is that the alphabet is `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*(){}_?` and the key is `5up3r_s3cr3t_k3y_f0r_1337h4x0rs_r1gh7?`. From this we can implement a simple decoder in python:

```py
>>> alphabet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*(){}_?'
>>> ct = '*fa4Q(}$ryHGswGPYhOC{C{1)&_vOpHpc2r0({'
>>> key = '5up3r_s3cr3t_k3y_f0r_1337h4x0rs_r1gh7?'
>>> # Subtracts index of ct[i] with index of key[i] and wraps around if < 0
>>> ''.join(alphabet[(alphabet.index(ct[i])-alphabet.index(key[i]))%len(alphabet)]for i in range(len(ct)))
'n00bz{4lph4num3r1c4l_1s_n0t_4_pr0bl3m}'
```

### Flag

```n00bz{4lph4num3r1c4l_1s_n0t_4_pr0bl3m}```