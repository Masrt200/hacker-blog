---
title: Castors-CTF
layout: post
---

![Banner](Snips/CASTOR/BANNER.jpg)

# Arithmetic

## Solution

We needed to solve simple arithmetic operations but we have little time and they get troll-some! Sounds like a challenge description heh?

Use Sympy, its the best for these types of challenges!

Here my code:

```python
from pwn import *
import re
from sympy import solve,Symbol,sympify

r=remote('chals20.cybercastors.com',14429)

print(r.recv())
r.sendline('')

mappings={'plus':'+','divided-by':'//','minus':'-','multiplied-by':'*','one':'1','two':'2','three':'3','four':'4','five':'5','six':'6','seven':'7','eight':'8','nine':'9','zero':'0'}
cn=0
while True:
	data=r.recv().decode()
	if 'castors' in data:
		print(data)
		break
	data=data.split(' ')
	print(data)
	for i in range(len(data)):
		if data[i] in mappings:
			data[i]=mappings[data[i]]
	data=' '.join(data)
	#print(data)
	num=re.findall(r'\d+',data)
	eqn=data[data.index(num[0]):data.index('?')]

	eq='x='+eqn
	#print(eqn)
	sympy_eq = sympify("Eq(" + eq.replace("=", ",") + ")")
	ans=solve(sympy_eq)
	print(ans)
	r.sendline(str(ans[0]))
	r.recvuntil('\n')
```

*castorsCTF(n00b_pyth0n_4r17hm3t1c5}*, pretty nooby!


# Jigglypuff's Song

### crypto

### Point: 479

### Description:
Can you hear Jigglypuff's song?

### Solution:

This was a really easy chall... but the random pixel stream on the image kinda confused me! Half of the time, I just kept staring at the image and did nothing!

All we had to do was a basic steganography attack! Using LSB to conceal data in images doesn't alter the image much, but MSB does a lot. As was the case here! Once that clicked, it was as easy as a sunny day!

```python
from PIL import Image

im=Image.open('chal.png')
pixels=im.load()

#print(im.size,im.mode)

search=[]

s=''
for i in range(im.height):
	for j in range(im.width):
		a=pixels[j,i]
		r=bin(a[0])[2:].zfill(8)[0]
		g=bin(a[1])[2:].zfill(8)[0]
		b=bin(a[2])[2:].zfill(8)[0]
		s+=r+g+b

print(bytes.fromhex(hex(int(s,2))[2:]))
```

*castorsCTF{r1ck_r0ll_w1ll_n3v3r_d3s3rt_y0uuuu}* ::The Damn Author trolls you twice... once in the trailing bits part and the entire concealed message also contains Rick's song twice!


# Bagel Bytes

### crypto

### Point: 444

### Description:
nc chals20.cybercastors.com 14420

## Solution:

It was a imple ECB-oracle attack nothing much! All you had to do was notice that!

```python
from pwn import *
import binascii

r=remote('chals20.cybercastors.com',14420)



def get_data(payload):
	r.recvuntil('choice: ')
	r.sendline('2')
	r.recvuntil('> ')
	r.sendline(payload)
	r.recvuntil(':\n')

	data=r.recvuntil('\n')[:-1]

	return data

flag=''
for i in reversed(range(0,48-len(flag))):
	payload='A'*i
	data=get_data(payload)

	print(len(data),i)
	for i in range(0,len(data),32):
		print(data[i:i+32])

	chars='castorsCTF}{I_L1k3_mua_1203456789abcdefghijklmnopqrstuvxyz?ABCDEFGHIJKLMNOPQRSTUVWXYZ'
	for j in chars:
		print('checking',j)
		check=payload+flag+j
		reply=get_data(check)
		if reply[:96]==data[:96]:
			flag+=j
			break

	print(flag)
```

*castorsCTF{I_L1k3_muh_b4G3l5_3x7r4_cr15pY}* -i- eazy-peasy!


# Amazon

### crypto

### Points: 489

### Description:
Are you watching the new series on Amazon?

## Solution:

The hint was infront of us for this one! Amazon--> Amazon Prime --> Primes! Ascii representation of each character was multiplied by consecutive prime numbers!


```python
a=['2', '3', '5', '7', '11', '13', '17', '19', '23', '29', '31', '37', '41', '43', '47', '53', '59', '61', '67', '71', '73', '79', '83', '89', '97', '101', '103', '107', '109', '113', '127', '131', '137', '139', '149', '151', '157', '163', '167', '173', '179', '181', '191', '193', '197', '199', '211', '223', '227', '229']
b=['198', '291', '575', '812', '1221', '1482', '1955', '1273', '1932', '2030', '3813', '2886', '1968', '4085', '3243', '5830', '5900', '5795', '5628', '3408', '7300', '4108', '10043', '8455', '6790', '4848', '11742', '10165', '8284', '5424', '14986', '6681', '13015', '10147', '7897', '14345', '13816', '8313', '18370', '8304', '19690', '22625']

for i in range(len(b)):
	print(chr(int(b[i])//int(a[i])),end='')
```

*castorsCTF{N0_End_T0d4y_F0r_L0v3_I5_X3n0n}*, how did the author expect us to find this before space were given in the ciphertext!


# 0x101 Dalmations

### crypto

### Points: 496

### Description:
Response to Amazon: Nah, I only need to be able to watch 101 Dalmatians.

## Solution:

It was exact the Amazon chall with one extra step! You couldn't have possibly solved it without solving Amazon!

All we have to do was take the primes multiply them by the ascii representation of the chars, and take its modulus with 0x101!

> (char\*prime)%0x101

It just bruteforce to reverse it! Could have used modular inverses but eh!

```python
p=['2', '3', '5', '7', '11', '13', '17', '19', '23', '29', '31', '37', '41', '43', '47', '53', '59', '61', '67', '71', '73', '79', '83', '89', '97', '101', '103', '107', '109', '113', '127', '131', '137', '139', '149', '151', '157', '163', '167', '173', '179', '181', '191', '193', '197', '199', '211', '223', '227', '229','239', '241' ,'251' ,'257', '263', '269','271' ,'277' ,'281' ,'283' ,'293' ,'307' ,'311','313','317']

a=['c6', '22', '3d', '29', 'c1', 'c5', '9c', 'f5', '85', 'e7', 'd7', '0e', '46', 'e6', '21', 'e7', 'dd', '8d', 'db', '43', 'a0', '34', '77', '04', '7f', '32', '13', '8c', 'c9', '01', '65', '78', '5f', 'c0', '14', '8e', '33', 'bf', 'bc', '02', '21', '79', 'e1', '5d', 'd3', '46', 'e0', 'ca', 'ee', '72', 'c2', '26', '38']
m=257
flag=''
for i in range(len(a)):
	for j in range(30,150):
		if (j*int(p[i]))%m==int(a[i],16):
			#print(j)
			#print(chr(j))
			flag+=chr(j)
	#print('\nlolol')

print(flag)
```

*castorsCTF{1f_y0u_g07_th1s_w1th0u7_4ny_h1n7s_r3sp3c7}*. I needed a lot of hints, so no respect for me I guess!


