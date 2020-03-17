---
title: Angstromctf
layout: post
---
# Keysar

### Crypto

### Points: 40

### Description:
Hey! My friend sent me a message... He said encrypted it with the key ANGSTROMCTF.

He mumbled what cipher he used, but I think I have a clue.

Gotta go though, I have history homework!!

*agqr{yue_stdcgciup_padas}*

>Hint: Keyed caesar, does that even exist??

## Solution:

The hint itself gives us the solution. Yes, Keyed Caeser cipher exists. It involves changing the alphabet just like in substitution cipher... The new alphabet would be *ANGSTROMCFBDEHIJKLPQUVWXYZ*

Using Rumkin.com here... 

![Bored](Snips/ANGS/KEYSAR.PNG)

There's yr easy flag on the 0th shift... *actf{yum_delicious_salad}*


# Reasonably Strong Algorithm

### Crypto

### Points: 70

### Description:
[RSA](https://files.actf.co/92ffe9027ea6bbe7c319eb755afb588f7165d3737b58cbf1d1fe361cbdb32b7c/rsa.txt) strikes again!

>Hint: After you get a number, you still have to get a flag from that.

## Solution:

Duh, its RSA... not Russia but RSA. From the link we gets this no.s from the website,

![Shiver](Snips/ANGS/RSA.PNG)

Well putting them in a file and giving it to my very own project, [D3crypt0r](https://github.com/Masrt200/WoC2k19)...

![Putin](Snips/ANGS/RSA2.PNG)

There you have it, *actf{10minutes}* is your flag. Enjoy!


# Wacko Images

### Crypto

### Points: 90

### Description:
How to make hiding stuff a e s t h e t i c? And can you make it normal again? [enc.png](https://files.actf.co/1ef5c3f44f52d62ea8b19e61d9eb6ef11a75c4eec88e1c086efe01e290439223/enc.png) [image-encryption.py](https://files.actf.co/1ef5c3f44f52d62ea8b19e61d9eb6ef11a75c4eec88e1c086efe01e290439223/enc.png)

The flag is actf{x#xx#xx_xx#xxx} where x represents any lowercase letter and # represents any one digit number.

## Solution:

We are given image pixelated image which seems not but loads of junk, maybe its encrypted? 

![pack](Snips/ANGS/WACKO1.PNG)

The code provided in the link gives us some intuition as to how it was done...

```python
from numpy import *
from PIL import Image

flag = Image.open(r"flag.png")
img = array(flag)

key = [41, 37, 23]

a, b, c = img.shape

for x in range (0, a):
    for y in range (0, b):
        pixel = img[x, y]
        for i in range(0,3):
            pixel[i] = pixel[i] * key[i] % 251
        img[x][y] = pixel

enc = Image.fromarray(img)
enc.save('enc.png')
```

Oh, so each pixel of the original image was encrypted by multiplying with a value in the key and taking its mod with 251... should be pretty easy to reverse right?

I wrote this script where it searches for the original pixel...

```python
from PIL import Image
enc=Image.open('enc.png')
pixels=enc.load()

key = [41, 37, 23]

flag=Image.new('RGB',enc.size)
pixy=flag.load()

for i in range(enc.width):
	for j in range(enc.height):
		pos=pixels[i,j]
		pix=[]
		for k in range(3):
			for l in range(1,100):
				sum=251*l+pos[k] #tries various values
				if sum%key[k]==0:
					pix.append(sum//key[k])
					break
		lix=(pix[0],pix[1],pix[2])
		pixy[i,j]=lix

flag.show()
flag.save('flag.png')		
```

This image pops up...

![nodd](Snips/ANGS/WACKO2.PNG)

Henceforth you get *actf{m0dd1ng_sk1llz}*


# The Magic Word

### Web

### Points: 20

### Description:
[Ask and you shall receive](https://magicword.2020.chall.actf.co/)...that is as long as you use the magic word.

>Hint: Change "give flag" to "please give flag" somehow

## Solution:

The site gives us a simple...
![dena](Snips/ANGS/MAGIC1.PNG)

The description says that we need to ask the magic word and the hint clearly tells us what it is... but first lets look at the source code.

![source](Snips/ANGS/MAGIC2.PNG)

Nothing much interesting but the above part clearly tells us that we need to send the message 'please give flag' as a /flag?msg='\*' and we will get our output. It also says its every letter is X-ORed by 0xf.

Try python won't I?

```python
import requests

url='https://magicword.2020.chall.actf.co/flag?msg=please give flag'
response=requests.get(url)
content=response.text

print(content)
```

So simple right, we get *nl{it\>a\|<\l8P\\<\c<\b<\a{Pf|Pv?z}Pm\<|{Pi}f\<\akr* Xorring it again with 0xf we get... *actf{1nsp3c7_3l3m3nt_is_y0ur_b3st_fri3nd}* as our flag.


# ws1

### Misc

### Points: 30