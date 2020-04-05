---
title: VIRSECCON CTF
layout: post
---

![Banner](Snips/VSCCTF/banner.PNG)


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

Then let's use the **Tesseract** no not the one with the time-stone... It's a Linux tool... Install it in Kali using *sudo apt-get install tesseract-orc*!

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