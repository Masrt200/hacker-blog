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

So simple right, we get nl\{it>a\|<l8P<c<b<a\{Pf|Pv?z}Pm<\|\{Pi\}f<akr
 Xorring it again with 0xf we get... *actf{1nsp3c7_3l3m3nt_is_y0ur_b3st_fri3nd}* as our flag.


# ws1

### Misc

### Points: 30

### Description:
Find my password from [this recording](https://files.actf.co/66f84b04a2631daadc929b58386bb42a746ec51a4664c2eefde569ce3ece2164/recording.pcapng) (:

## Solution:

This is a simple wireshark... the title ws short for wireshark and the downloaded pcap file points to those.

Opening wireshark, we find the flag by following the TCP stream 0 (tcp.stream eq 0)

![tcp](Snips/ANGS/WIRE1.PNG)

*actf{wireshark_isn't_so_bad_huh-a9d8g99ikdf}* 's the flag. Simple, right!'


# ws2

### Misc

### Points: 80

### Description:
No ascii, not problem :)

[recording.pcapng](https://files.actf.co/3d02af8f9607577cbf7565844a1343efe1885c5428905499e86ebfb5197be21f/recording.pcapng)

## Solution:

This one was nice and new. Opening it, you see a HTTP protocol having a JPEG image. When you follow it, we find it the raw hex data of the image. We just need to extract that and open it as an image.

![WIRE2](Snips/ANGS/WIRE2.PNG)

There maybe many more methods but I did this. If you come by any other, let me know. I saved the image as a .jpg file. But we still have that chunk data of unwanted text...

Now open the image using ghex and select the unwanted hex data (before hex values FF D8) and use edit>cut

![GHEX](Snips/ANGS/WIRE21.PNG)

Do the same for the bottom part start... and the save the file.

Now if you use file command on the jpeg file;

![file](Snips/ANGS/WIRE22.PNG)

Now our file is a jpeg image in the proper format. Opening it,

![one_punch](Snips/ANGS/WIRE23.PNG)

We have our flag, *actf{ok_to_b0r0s-4809813}*


# msd

### Misc

### Points: 140

### Description:
You thought Angstrom would have a stereotypical LSB challenge... You were wrong! To spice it up, we're now using the [Most Significant Digit](https://files.actf.co/4be3ae7e90eb6cd33df6a933672a6d3c0041aefab146daf5e6c9f71a398fd15d/public.py). Can you still power through it?

Here's the [encoded image](https://files.actf.co/8d2542479944383b4bfcac63681c8afb93b26187ac012389e0caac6bba45d035/output.png), and here's the [original image](https://files.actf.co/4ffd75debba1c5add31ef07293eebc24e799d2a2a8c51632540478410bb43b08/breathe.jpg), for the... well, you'll see.

Important: Don't use Python 3.8, use an older version of Python 3!

>Hint1: Look at the difference between the original and what I created!

>Hint2: Also, which way does LSB work?

## Solution:

This was a fun challenge. You need to open both files to analyse the pixel data of how the original file was changed using the code. Basically the first character string values of every pixel value was replaced by a no. which the flag data converted to ascii. ex: if the flag is *AA* its ascii becomes *6565* and the first pixel holds data *(85,42,78)* then output pixel becomes *(65,52,68)* see how?! However, there were some peculiar cases as if the pixel data is >100 and after the replacing the first letter by a no. greater than 2 or in case where by changing, the pixel values becomes greater than 255... rather than giving an error its turns the value back to 255, we need to account for that. Here's my code;

```python
from PIL import Image
import random

im=Image.open('output.png')
im1=Image.open('breathe.jpg')
pixels=im.load()
pixy=im1.load()

a,b=im.size

ans=''
for j in range(im.height):
	for i in range(im.width):
		a=pixels[i,j]
		b=pixy[i,j]
		for k in range(3):
			if len(str(a[k]))<len(str(b[k])):
				ans+='0'
			elif a[k]==255 and b[k] not in [255]:
				ans+=str(random.randint(0,9)) #chooses random int incase the value is 255
			else:
				ans+=str(a[k])[0]
ans1=''
i=0 
while i<len(ans): #breaks the large no. chunk into ascii format data like 109 48 44 111 105
	if ans[i] not in ['1']:
		c=ans[i:i+2]
		i+=2
	elif ans[i]=='1':
		c=ans[i:i+3]
		i+=3

	ans1+=c+' '
j=0
ans2=''
for i in range(len(ans1)):
	if ans1[i]==' ':
		ans2+=chr(int(ans1[j:i]))
		j=i+1

print(ans2)
```

The answer you get is a lot of data... if you grep or find for the flag using the keyword *actf* you will eaily get it on the 3rd or 4th search result.

*actf{inhale_exhale_ezpz-12309biggyhaby}* UwU!

