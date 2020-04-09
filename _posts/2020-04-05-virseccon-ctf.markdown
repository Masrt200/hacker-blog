---
title: VIRSECCON CTF
layout: post
---

![Banner](Snips/VSCCTF/banner.png)


# Polybius

### Points: 70

### Description:
Polybius is hosting a party, and you're invited. He gave you his number; be there o be square!

Note, this flag is not in usual flag format.

## Solution:

We get these numbers,

`413532551224514444425111431524443435454523114523114314`

These are in-fact kinda like coordinates to a square filled with letter, Polybius here is the hint!

Go to [dcode.fr](https://www.dcode.fr/polybius-cipher) and then polybius square cipher!... 

*POLYBIUSSQUAREISNOTTHATHARD* is the flag!


# Classic

### Crypto

### Points: 80 

### Description:
My friend told me this "was a classic", but these letters and numbers dont make any sense to me. I didn't know this would be a factor in our friendship!

## Solution:

```
n:77627938360345301510724699969247652387657633828943576274039402978346703944383
e:65537
c:62899945974090753231979111677615029855602721049941681356856158761811378918268
```

Classic RSA stuff, one can easily tell! I used my own [D3crypt0r](https://github.com/Masrt200/WoC2k19). Give it a nice look :)

![RSA](Snips/VSCCTF/CLASSIC.PNG)

Ezy Pzy right?! *LLS{just_another_rsa_chal}* is the flag!


# Chief Executive Officer

### Crypto

### Points: 70

### Description:
I found this weird file a while ago... I've tried to operate with it but can't seem to get it to work. I've tried the whole range of possibilities!

## Solution:

I found this a bit weird but this chall had a lot less solves than even the RSA ones... Anyway, the file contained an entire junk of non-printable characters... hmm maybe just maybe we can bruteforce XOR them...!

```python
f=open('chief_executive_officer','rb')
data=f.read()

n=''
for j in data:
	n+=chr(137^ord(j))

print(n)
```

Yup that's all you needed, I brute-forced the contents of the file with single-byte ascii characters from 1 to 256 and found that there were readable English words on the 137th character... 

You get a lot of text but just grep LLS{.\*} and you will have your flag!

*LLS{you_are_the_ceo_of_xor}* Measly meh~


# Hot Dog

### Crypto

### Points: 100

### Description:
This isn't a problem with the grill.

## Solution:

The decription doesn't say much but opening the file gives us a lot. The size of `e` by the look of it is very close to that of `n`. So this must solved by a [Weiner's attack](https://en.wikipedia.org/wiki/Wiener%27s_attack) on RSA...

```
n:609983533322177402468580314139090006939877955334245068261469677806169434040069069770928535701086364941983428090933795745853896746458472620457491993499511798536747668197186857850887990812746855062415626715645223089415186093589721763366994454776521466115355580659841153428179997121984448771910872629371808169183
e:387825392787200906676631198961098070912332865442137539919413714790310139653713077586557654409565459752133439009280843965856789151962860193830258244424149230046832475959852771134503754778007132465468717789936602755336332984790622132641288576440161244396963980583318569320681953570111708877198371377792396775817
c:387550614803874258991642724003284418859467464692188062983793173435868573346772557240137839436544557982321847802344313679589173157662615464542092163712541321351682014606383820947825480748404154232812314611063946877021201178164920650694457922409859337200682155636299936841054496931525597635432090165889554207685
```

Once again using my tool [D3crypt0r](https://github.com/Masrt200/WoC2k19)... it has quite a bunch of crypto-related stuff!

![RSA](Snips/VSCCTF/CLASSIC.PNG)

*LLS{looks_like_weiners_on_the_barbecue}* is the dish!


# Old Monitor

### Crypto

### Points: 135

### Description:
I have this old CRT monitor that I use for my desktop computer. It crashed on me and spit out all these weird numbers...

# Solution:

A image file with huge no.s on them... first we need to extract those... Now we may do this by hand which will involve lots of errors or do the techy way!

First scale up the file so that the pixels are better readable...

```convert old_monitor.png -scale 300% pc.png```

Then let's use the **Tesseract** no not the one with the time-stone... It's a Linux tool... Install it in Kali using *sudo apt-get install tesseract-ocr*!

Now let's extract the data...

```tesseract pc.png chinese```

As you can see...,

![HASTAD](Snips/VSCCTF/MONITOR1.PNG)

It did quite well apart from one or two characters which we can do manually... Now the important it seems like we have to perform a Hastad-Broadcast attack or use the Chinese Remainder theorem to solve this!

```python
from sympy import mod_inverse
import binascii

def root3rd(x):
    y, y1 = None, 2
    while y!=y1:
        y = y1
        y3 = y**3
        d = (2*y3+x)
        y1 = (y*(y3+2*x)+d//2)//d
    return y 

def hastad(n1,n2,n3,c1,c2,c3):
	
	N=n1*n2*n3
	N1=n2*n3
	N2=n3*n1
	N3=n1*n2

	#print(N1,N2,N3)

	b1=mod_inverse(N1,n1)
	b2=mod_inverse(N2,n2)
	b3=mod_inverse(N3,n3)

	#print(b1,b2,b3)

	t1=c1*N1*b1
	t2=c2*N2*b2
	t3=c3*N3*b3

	C=(t1+t2+t3)%N

	M=root3rd(C)
	m=binascii.unhexlify(hex(M)[2:].rstrip('L'))
	return m


n1=REDACTED 
n2=REDACTED 
n3=REDACTED 
c1=REDACTED 
c2=REDACTED 
c3=REDACTED 

print(hastad(n1,n2,n3,c1,c2,c3))
```

I used this code... pretty long right... you will soon find it in my D3crypt0r too!

*LLS{the_chinese_remainder_theorem_is_so_cool}* is the sweet flag!


# CALC-UL8R

### Scripting

### Points: 150

### Description:
Texas Instruments latest new product: you! Connect with: `nc jh2i.com 50003`

## Solution:

This is pretty simple is you are asked to do say ten such problems by hand but its not ten, probably a hundred or so. We are given simple one-variate maths equation and we have to return the value of the variable. At first I thought of getting the variables and constants and then operating some-how but that would be too much of a hassle. Then I remembered about the Python Module `Sympy`. It's the best for solving equation which are written in human-readable format.

```python
rom sympy.solvers import solve
from sympy import Symbol,sympify
from pwn import *
import re
import time

s=remote('jh2i.com',50003)
data=s.recvuntil('\n\n')
while True:
	time.sleep(1)
	data=s.recv()
	print(data)
	if 'LLS' in data:
		break
	data=data.split(' ')
	data=data[:-2]
	data[-1]=data[-1][:-2]
	eq=''.join(data)
	sym_eq=sympify("Eq("+eq.replace("=",",")+")")
	val=solve(sym_eq)[0]
	print(val)
	s.sendline(str(val))
```

That's the script I used... It takes the equation and sends the result.

*LLS{sympy_to_solve_equations}* also nods to using **Sympy**


# Gammer 

### Scripting

### Points: 125 

### Description: 
It's only one letter away! Connect with: `nc jh2i.com 50012`

## Solution:

This netcat server checks for each pos of the input flag... if that position has the correct letter the its returns *Correct* else *Wrong*... so we just need to build up the flag from the ground up!

```python
from pwn import *
import re
import time
printable='LS}_{abcdefghijLklmnopqrstuvwxyzABCDEFGHIJKMNOPQRSTUVWXYZ0123456789'

flag='LLS{'

while 1:
	for i in printable:
		s=remote('jh2i.com',50012)
		s.recv()
		s.sendline(flag+str(i))
		#time.sleep(5)
		data=s.recvuntil('it!')
		#print(i,data)
		if 'WRONG' not in data:
			flag+=i
			print(flag)
			break
		else:
			print('0')
```

For some reason, my script was detecting the *Wrong* return messsage on the same connection, so I had to reset the connection everytime and start a new one...

It took some time but we finally we get the flag *LLS{bruteforce_with_a_hammer}*


# 2xPAD 

### Scripting

### Points: 100

### Description: 
The latest and greatest encryption scheme, "two-time pad" is twice as secure as one time pads. Check it out! 

## Solution:

This challenge gives us two *.png* files to work with. They both contain a jibberish mix of pixels... so they first thing that comes to mind is X-ORing them. Now, I know this is a scripting challenges but sometimes other tools are already there. Use *stegsolve* here...!

![PAD](Snips/VSCCTF/PAD.PNG)

*LLS{dont_reuse_your_keys}* is the flag!


# Pincode 

### Scripting

### Points: 75 

### Description:
This service needs a 4 digit pincode to authenticate... can you help me figure it out!?? Connect with: `nc jh2i.com 50031`


## Solution:

Now this was albiet, quite a easy challenge which took me an unfathomable amount of time. The server needed a 4-digit pass code, which when would give us the flag! Now at first I thought about a no. from the range *1000-9999*, normal right?! But even after iterating through it, I couldn't get the flag. Then something clicked and it tried from *0000-0999* and ho and behold it was just *0037* :C

```python
from pwn import *
i=9999
while i>=0:
	s=remote('jh2i.com',50031)
	print(s.recv())
	e=str(i).zfill(4)
	s.sendline(e)
	a=s.recv()
	print(e,a)
	if 'LLS' in a:
		break

	i-=1
```

*LLS{for_i_in_0000_to_9999}*  is the shitty result!


# Quick Run

### Scripting

### Points: 75 

### Description:
You gotta go fast!

## Solution:

The challenge gives us a zip file which has many QR-codes... now we can to it the oldfashioned way or use *zbarimg*

I unzipped the file, removed it and used... `zbarimg * -q`

We get a bunch of ascii codes, convert them to characters and we get the flag.

*LLS{zbarimg_makes_qrcodes_easy}*


# 2048

### Scripting

### Points: 75

### Description:
cGx6aGVscG1l

## Solution:

This chall gives us a text file which contains base64 encoded text decoding it once doesn't help so there must be multiple layers of base64 encodings

```python
import base64

f=open('2048','r')
data=f.read()
f.close()
for i in range(100):
	try:
		data=base64.b64decode(data)
	except:
		print(data)
		break
```

Quickly put up this scipt... *LCSC{i_hope_you_didnt_use_asciitohex.com}* is the flag~


# Whitepages

### Stego

### Points: 80

### Description:
I stopped using yellow pages and moved onto white-pages... but the book they gave me is all blank.

## Solution:

Oh, Nostalgia is hard! This is the exact same problem and probably the same title and description used for one problem in PicoCTF 19... basically the text file uses two types of blank_space characters in it. So we take either as 0 or 1 and we should get the answer, after converting the binary to decimal

```python
from pwn import *
f=open('white.txt','rb')
data=f.read()
data  = data.replace('e28083'.decode('hex'), '0').replace(' ', '1')
print(unbits(data))
```

See, its that simple... *LLS{whitespace_steganography_in_the_empty_space}* is the flag!


# Stegosaurus

### Stego

### Points: 70

### Description:
Scientists are struggling with a new mystery: we thought the dinosaurs were gone, but this one has returned! Hmm... can you solve this mystery?

## Solution:

Simple `stegsolve` problem that's it!

![steg](Snips/VSCCTF/STEG.PNG)

*LLS{you_stegsolved_the_mystery}*


# Winter Wonderland

### Stego

### Points: 80

### Description:
It’s the holiday season! But hmm… they must be hiding something under all that cheer!

## Solution:

A bit misleading... at first I thought of rockstar but then after opening the file in sublime text... I saw all the trailing whitespaces and tabspaces... this could mean only *stegsnow* was used!

Just use `stegsnow winter_wonderland.txt` and we were done!

*LLS{let_it_snow_baby_let_it_reindeer}*


