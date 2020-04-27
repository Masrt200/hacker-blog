---
title: Houseplant CTF
layout: post
---

![Banner](Snips/HOUSEPLANT/BANNER.png)


# CH3COOH

### Crypto

### Points:50

### Description:
None
>Hint: Short keys are never a good thing in cryptography.

## Solution:

We are given a large passage of encrypted ciphertext. Hmm, what could it be... The title says CH3COOH... which is the chemical formula for Vinegar. Vinegar, Vinegar... Vigenere. Its a vignere cipher.

Using my self written code from my [D3crypt0r](https://github.com/Masrt200/WoC2k19), after saving the text to a file!

![CH31](Snips/HOUSEPLANT/CH31.png)

Vigenere cipher without a key is done by frequnecy analysis of letters in an alphabet... doing so we get the key `tony`... Now onto the decryption!

![CH32](Snips/HOUSEPLANT/CH32.png)

Simple, right! *rtcp{vinegar_on_my_fish_and_chips_please}* was the flag!


# "fences are cool unless they're taller than you" - tida

### Crypto

### Points:50

### Description:
They say life's a roller coaster, but to me, it's just jumping over fences.

## Solution:

We are given a ciphertext with english-letter alphabet, bracesa and numbers in any jumbled order. Well, any hint... the title is suffice. `fences` here denote the railfence cipher.

I solved this too using my [D3crypt0r](https://github.com/Masrt200/WoC2k19)... plz do check it out!

![RAIL](Snips/HOUSEPLANT/RAIL.png)

You can see clearly that `solution 2`; which means using 3 rails, we get a close enough answer that makes sense.

*rtcp{ask_tida_about_rice_washing}* is the flag!


# Returning Stolen Archives

### Crypto

### Points:50

### Description:
So I was trying to return the stolen archives securely, but it seems that I had to return them one at a time, and now it seems the thieves stole them back! Can you help recover them once and for all? It seems they had to steal them one at a time...

>Hint: Well you sure as hell ain't going to solve this one through factorization.

## Solution:

We are given a python file which is probably... how the data in intercepted.txt is encrypted. The title gives a clear hint that `R S A` is used!

```python
p = [redacted]
q = [redacted]
e = 65537
flag = "[redacted]"

def encrypt(n, e, plaintext):
  print("encrypting with " + str(n) + str(e))
  encrypted = []
  for char in plaintext:
    cipher = (ord(char) ** int(e)) % int(n)
    encrypted.append(cipher)
  return(encrypted)

n = p * q
ct = encrypt(n, e, flag)
print(ct)
```

As per the encrypting script... RSA encryption has been performed on each character of the plaintext. No other information on bitsize of the primes or `n` in general.

Hmm... looking at the intercepted.txt file... we see N is sufficiently large and there's the hint that we can't solve it through factorization! Hmm...

```python
n=[redacted]
e=65537
ct=[redacted]

plaintext=''
for i in ct:
	for j in range(1,256):
		if pow(j,e,n)==i:
			plaintext+=chr(j)

print(plaintext)
```

Heh! All there was to do was take every ascii character and check whether it matches the corresponding ciphertext or not!

*rtcp{cH4r_bY_Ch@R!}* for peace-keeping sake!


# Rivest Shamir Adleman

### Crypto

### Points: 338

### Description:
A while back I wrote a Python implementation of RSA, but Python's really slow at maths. Especially generating primes.

>Hint: There are two possible ways to get the flag ;-)

## Solution:

Another straight giveway... **Rivest Shamir Aldeman** is what RSA stands for #themakers! The `chall.7z` file given, had a lot more things than expected... easy to have your head spining!

![RSA](Snips/HOUSEPLANT/RSA1.png)

Now `encrypt.py` and `decrypt.py` is where my eyes went first... coz ofc. Both files... didn't give much, rather than getting the keys from from a file and performing encryption and decryption respectively. One key note is that a verification string was added *'VERIFICATION-UpTheCuts-END\n'* , which could be important if we decrypt using the `decrypt.py` file.

**OK**, enough of this crap! The real deal is the `generate_keys.py`!!

First we see two primality test funcs... one a general one and other a Miller_Rabin Test! There are a few more funcs... but the real hint lies at `line 115`...

![component](Snips/HOUSEPLANT/RSA2.png)

You see, while generating primes... this func generates two primes but it changes the first prime to a value read from `component.txt` ... if a certain `primes.json` doesn't exists otherwise it reads the primes from it!... Now since, there isn't any primes.json we may assume that this is indeed the case!

So, its goes easy from here... we get one prime from `component.txt`, the public key from `public-key.json` and the ciphertext from `secrets.txt.enc` and play around. 

```python
import json
from Crypto.Util.number import *

c=int(open('secrets.txt.enc').read()[2:],16)
key=json.loads(open('public-key.json').read())

p=int(open('component.txt').read())
q=key['n']//p

tot=(p-1)*(q-1)
d=inverse(key['e'],tot)

m=pow(c,d,key['n'])
print(long_to_bytes(m))
```

There you go; we get the nice message as,

![Key](Snips/HOUSEPLANT/RSA3.png)

*rtcp{f1xed_pr\*me-0r_low_e?}* is the flag! 

Now as the hint says there is surely another way to find this `flag`. The flag reads *fixed prime or low e?*... I am pretty sure my method summarizes the fixed prime one! I wasn't able to look into the other because of the ongoing contest but do give it a try! I will too!


# Rainbow Vomit

### Crypto

### Points:535

### Description:
o.O What did YOU eat for lunch?!

>Hint: Replace spaces in the flag with { or } depending on their respective places within the flag.

>Hint: Hues of hex

>Hint: This type of encoding was invented by Josh Cramer.

## Solution:

A quick search of *Josh Cramer & hues of hex* as the hint says gave enough us information about his [encoding scheme](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=2ahUKEwi__PiEnojpAhUGXn0KHUYACLgQFjABegQIAxAB&url=https%3A%2F%2Fwww.boxentriq.com%2Fcode-breaking%2Fhexahue&usg=AOvVaw1IuPaMKOW7bF1jHQMoqGdt)

He used pixels of differnt colours arranged in 2x3 rectangles to encode the letters and numbers

![Rainbow](Snips/HOUSEPLANT/RAINBOW1.png)

Now this basically tells us what to do... lets look for a image to text decoder... oh wait, no ... NO... **NO!!**. There isn't any or one that I could find. So yea... fold up your sleeves and get ready to code! coz the image provided to us, as much as we can zoom would be painful to do it manually! 

At 2000% zoom in a linux machine... we see this,

![Rainbow2](Snips/HOUSEPLANT/RAINBOW2.png)

The image is 104X34 in dimensions and we can tell that there's a 2 pixel width of border... that would make our data 100x30 in dimensions, which fits as mulitples of our base rectangle dimensions!

```python
from PIL import Image

'''
taking input along lines
'''

# striping surrounding white pixels

'''
im=Image.open('output.png')

pixels=im.load()

im1=Image.new('RGB',(100,30))
pixy=im1.load()

for i in range(2,32):
	for j in range(2,102):
		pixy[j-2,i-2]=pixels[j,i]

im1.save('better.png')
'''

colour={"B":(0,0,0),
"W":(255,255,255),
"G":(128,128,128),
"r":(255,0,0),
"g":(0,255,0),
"b":(0,0,255),
"y":(255,255,0),
"c":(0,255,255),
"m":(255,0,255)
}

a=[colour["m"],colour["r"],colour["g"],colour["y"],colour["b"],colour["c"]]
b=[colour["r"],colour["m"],colour["g"],colour["y"],colour["b"],colour["c"]]
c=[colour["r"],colour["g"],colour["m"],colour["y"],colour["b"],colour["c"]]
d=[colour["r"],colour["g"],colour["y"],colour["m"],colour["b"],colour["c"]]
e=[colour["r"],colour["g"],colour["y"],colour["b"],colour["m"],colour["c"]]
f=[colour["r"],colour["g"],colour["y"],colour["b"],colour["c"],colour["m"]]
g=[colour["g"],colour["r"],colour["y"],colour["b"],colour["c"],colour["m"]]
h=[colour["g"],colour["y"],colour["r"],colour["b"],colour["c"],colour["m"]]
i=[colour["g"],colour["y"],colour["b"],colour["r"],colour["c"],colour["m"]]
j=[colour["g"],colour["y"],colour["b"],colour["c"],colour["r"],colour["m"]]
k=[colour["g"],colour["y"],colour["b"],colour["c"],colour["m"],colour["r"]]
l=[colour["y"],colour["g"],colour["b"],colour["c"],colour["m"],colour["r"]]
m=[colour["y"],colour["b"],colour["g"],colour["c"],colour["m"],colour["r"]]
n=[colour["y"],colour["b"],colour["c"],colour["g"],colour["m"],colour["r"]]
o=[colour["y"],colour["b"],colour["c"],colour["m"],colour["g"],colour["r"]]
p=[colour["y"],colour["b"],colour["c"],colour["m"],colour["r"],colour["g"]]
q=[colour["b"],colour["y"],colour["c"],colour["m"],colour["r"],colour["g"]]
r=[colour["b"],colour["c"],colour["y"],colour["m"],colour["r"],colour["g"]]
s=[colour["b"],colour["c"],colour["m"],colour["y"],colour["r"],colour["g"]]
t=[colour["b"],colour["c"],colour["m"],colour["r"],colour["y"],colour["g"]]
u=[colour["b"],colour["c"],colour["m"],colour["r"],colour["g"],colour["y"]]
v=[colour["c"],colour["b"],colour["m"],colour["r"],colour["g"],colour["y"]]
w=[colour["c"],colour["m"],colour["b"],colour["r"],colour["g"],colour["y"]]
x=[colour["c"],colour["m"],colour["r"],colour["b"],colour["g"],colour["y"]]
y=[colour["c"],colour["m"],colour["r"],colour["g"],colour["b"],colour["y"]]
z=[colour["c"],colour["m"],colour["r"],colour["g"],colour["y"],colour["b"]]

A0=[colour["B"],colour["G"],colour["W"],colour["B"],colour["G"],colour["W"]]
A1=[colour["G"],colour["B"],colour["W"],colour["B"],colour["G"],colour["W"]]
A2=[colour["G"],colour["W"],colour["B"],colour["B"],colour["G"],colour["W"]]
A3=[colour["G"],colour["W"],colour["B"],colour["G"],colour["B"],colour["W"]]
A4=[colour["G"],colour["W"],colour["B"],colour["G"],colour["W"],colour["B"]]
A5=[colour["W"],colour["G"],colour["B"],colour["G"],colour["W"],colour["B"]]
A6=[colour["W"],colour["B"],colour["G"],colour["G"],colour["W"],colour["B"]]
A7=[colour["W"],colour["B"],colour["G"],colour["W"],colour["G"],colour["B"]]
A8=[colour["W"],colour["B"],colour["G"],colour["W"],colour["B"],colour["G"]]
A9=[colour["B"],colour["W"],colour["G"],colour["W"],colour["B"],colour["G"]]
space=[colour["W"],colour["W"],colour["W"],colour["W"],colour["W"],colour["W"]]
space2=[colour["B"],colour["B"],colour["B"],colour["B"],colour["B"],colour["B"]]
comma=[colour["W"],colour["B"],colour["B"],colour["W"],colour["W"],colour["B"]]
period=[colour["B"],colour["W"],colour["W"],colour["B"],colour["B"],colour["W"]]

letters={'a':a,'b':b,'c':c,'d':d,'e':e,'f':f,'g':g,'h':h,'i':i,'j':j,'k':k,'l':l,'m':m,'n':n,'o':o,'p':p,'q':q,'r':r,'s':s,'t':t,'u':u,'v':v,'w':w,'x':x,'y':y,'z':z,'0':A0,'1':A1,'2':A2,'3':A3,'4':A4,'5':A5,'6':A6,'7':A7,'8':A8,'9':A9,' ':space,',':comma,'.':period}


im=Image.open("better.png")
pixels=im.load()

#print(im.size,im.height)

#main code... 
hexhue=[]

for I in range(0,im.height,3):
	for J in range(0,im.width,2):
		partpix=[]
		for K in range(3):
			for L in range(2):
				partpix.append(pixels[J+L,I+K])

		hexhue.append(partpix)

#print(hexhue)
plaintext=''
for A in hexhue:
	for B in letters:
		if letters[B]==A:
			plaintext+=B

print(plaintext)
```

I dont know if there was an easy way... took about an hour to just gets the inputs correct! The main code is basically looping through all the pixels as blocks of the given dimension! The only hard part was inputting the pixel value of every character... luckily there was a pattern... sigh!

You get this as the plaintext...

```
there is such as thing as a tomcat but have you ever heard of a tomdog. this is the most important question of our time, and unfortunately one that may never be answered by modern science. the definition of tomcat is a male cat, yet the name for a male dog is max. wait no. the name for a male dog is just dog. regardless, what would happen if we were to combine a male dog with a tomcat. perhaps wed end up with a dog that vomits out flags, like this one rtcp should,fl5g4,b3,st1cky,or,n0t
```

Funny right?! anyway *rtcp{should,fl5g4,b3,st1cky,or,n0t}* is the flag!


# Sizzle

### Crypto

### Points:50

### Description:
Due to the COVID-19 outbreak, we ran all out of bacon, so we had to use up the old stuff instead. Sorry for any inconvenience caused...

>Hint: Wrap your flag with rtcp{}, use all lowercase, and separate words with underscores.

>Hint: Is this really what you think it is?

## Solution:

The hint here lies in the description as `bacon`. But the ciphertext file has something like morse-code! Simple... replace the `.`'s for `a`'s and `-`'s for `b`'s! 

Next I used my [D3crypt0r](https://github.com/Masrt200/WoC2k19) to solve it.

![BACON](Snips/HOUSEPLANT/Rainbow2.png)

We clearly see the ciphertext! Removing some spaces and adding some underscores later...

*rtcp{bacon_but_grilled_and_morsified}* was my dinner!


# Parasite

### Crypto

### Points: 784

### Description:
paraSite Killed me A liTtle inSide

Flag: English, case insensitive; turn all spaces into underscores

>Hint: Make sure you keep track of the spacing- it's there for a reason

## Solution:

The file provided contains another morse-code... which decodes to this
`JDU MEK KDF PUF PTK JEF LU GEUK CHK KUW FU BE`... minding the spaces, as the hint says!

Now what?! I couldn't comeup with anything for a while until I read the description carefully. Some letters are capitalized randomly... they make up the word `SKATS`. A quick google search revealed `SKATS-Standard Korean Alphabet Transliteration System`! So yea... SKATS... Korea... it all made sense!

Reading the [wikipedia](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=5&cad=rja&uact=8&ved=2ahUKEwjj2LnMqojpAhUowTgGHWIlDBoQFjAEegQIDRAF&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSKATS&usg=AOvVaw21CmIEbulzyRq1qYcnDqby) article tells us how traditional Hangul script of South Korea was transliterated using SKATS to the english alphabet!

So, we need a SKATS decoder but there isn't one... so lets try writing out the Korean letters with the given mapping using a [Korean Keyboard](https://www.branah.com/korean)

It translites to something like this `희망은 진정한 기생충입니다` *p.s. I lost the actual message... I just translated back the flag*

Translating it to english gives us... `Hope is a true parasite`. Sigh... almost learnt the Korean alphabet system doing so! No offences!

*rtcp{Hope_is_a_true_parasite}* is the flag!


# Post-Homework Death

### Crypto

### Points: 570

### Description:
My math teacher made me do this, so now I'm forcing you to do this too.

Flag is all lowercase; replace spaces with underscores.

>Hint: When placing the string in the matrix, go up to down rather than left to right.

>Hint: Google matrix multiplication properties if you're stuck.

## Solution:

This was basically a maths question

We are given a 3x3 decoding matrix, and a array of 21 numbers. Since the hint says multiplication, what we need to do is arrange the 21 numbers in a **3x7** matrix and multiply it with the decoding matrix!
Also the hints states how to put the numbers in the matrix!

```python
import numpy as np

a=[37,36,-1,34,27,-7,160,237,56,58,110,44,66,93,22,143,210,49,11,33,22]
b=[[1.6,-1.4,1.8],[2.2,-1.8,1.6],[-1,1,-1]]
C=np.matrix(b)
D=np.matrix([a[::3],a[1::3],a[2::3]])

A=np.matmul(C,D)

plaintext=''
for i in range(7):
	for j in range(3):
		pos=round(A[j,i])
		plaintext+=chr(int(pos)+96)

print(plaintext)
```

This simple script gives the output... apparently we have to use A1Z26 cipher to decode the final result!

*rtcp{go_do_your_homework}* is the flag!


# Broken Yolks

### Crypto

### Points: 50

### Description:
Fried eggs are the best.
Oh no! I broke my yolk... well, I guess I have to scramble it now.

Ciphertext: smdrcboirlreaefd

>Hint: words are separated with underscores

## Solution:

This was solved by my team-mate and apparently is not a cipher. We just have to unscramble to letters of the ciphertext.

Honestly, It could be child's play if you are good at it!

*rtcp{scrambled_or_fried}* is my breakfast!




