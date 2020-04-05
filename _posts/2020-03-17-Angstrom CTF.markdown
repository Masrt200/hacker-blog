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

*actf{wireshark_isn't_so_bad_huh-a9d8g99ikdf}* 's the flag. Simple, right!


# ws2

### Misc

### Points: 80

### Description:
No ascii, not problem :)

[recording.pcapng](https://files.actf.co/3d02af8f9607577cbf7565844a1343efe1885c5428905499e86ebfb5197be21f/recording.pcapng)

## Solution:

This one was nice and new. Opening it, you see a HTTP protocol having a JPEG image. When you follow it, we find it the raw hex data of the image. We just need to extract that and open it as an image.

![WIRE2](Snips/ANGS/WIRE2.PNG)

There maybe many more methods but I did this. If you come by any other, let me know. I saved the image as a .jpg file. But we still have that chunk of unwanted text...

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


# Shifter

### Misc

### Points: 160

### Description:
What a strange challenge...

It'll be no problem for you, of course!

*nc misc.2020.chall.actf.co 20300*

>Hint: Do you really need to calculate all those numbers?

## Solution:
Connecting to the netcat server we get this message... 

*Solve 50 of these epic problems in a row to prove you are a master crypto man like Aplet123!
You'll be given a number n and also a plaintext p.
Caesar shift `p` with the nth Fibonacci number.
n < 50, p is completely uppercase and alphabetic, len(p) < 50
You have 60 seconds!*

Easy right! We need just get the ciphertext message and the n from the request and return the correct plaintext value. Obviously doing it manually will be impossible in the given time frame, so writing a script is necessary.

```python
from pwn import *
import re 
r = remote('52.207.14.64',20300) 
#r.send('Y')
#print(r.recv())
'''def fibonacci():
	fibo=[0,1]
	for i in range(48):
		fibo.append(fibo[i]+fibo[i+1])

	return fibo'''
  
def caeser(ciphertext,n):
	fibo=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986, 102334155, 165580141, 267914296, 433494437, 701408733, 1134903170, 1836311903, 2971215073, 4807526976, 7778742049]
	shift=fibo[n]%26
	alphabet='ABCDEFGHIJKLMNOPQRSTUVWXYZ'
	plaintext=''
	for i in ciphertext:
		plaintext+=alphabet[alphabet.index(i)-(26-shift)]

	return plaintext

def netcat():
	for i in range(51):
		data=r.recv()
		print(str(data))
		ciphertext=re.findall('[A-Z]+',str(data))[-1]
		if ciphertext=='': #sometimes the ciphertext is nothing and it returns an error
			r.sendline(None)
			continue
		n=int(re.findall('\d+',str(data))[-1])
		plaintext=caeser(ciphertext,n)
		r.sendline(plaintext)

netcat()
#print(fibonacci())
```

I used the above func fibonacii to calculate the fibonacci series till the 50th element and then stored them directly in the variable. The hints points to not calculating `n`, it may mean this or something else? I am not exactly sure... Let me know if you get it!

So yea.. run the above script where I used *remote* connection function from *pwntools*. There may be other methods as well but I like this...

*actf{h0p3_y0u_us3d_th3_f0rmu14-1985098}* is the flag! 


# One Time Bad

### Crypto

### Points: 100

### Description:
My super secure service is available now!

Heck, even with [the source](https://files.actf.co/c9a06f7a95b1b6c75fd4a04dd2e557df1843186d5d744ce858cd7c98ecf2e05d/server.py), I bet you won't figure it out.

*nc misc.2020.chall.actf.co 20301*

## Solution:

This one had me banging my heads for a while. I solved it after the CTF was over with help from John_Hammond do check out his channel [youtube](https://www.youtube.com/user/RootOfTheNull).

```python
import random, time
import string
import base64
import os

def otp(a, b):
	r = ""
	for i, j in zip(a, b):
		r += chr(ord(i) ^ ord(j))
	return r


def genSample():
	p = ''.join([string.ascii_letters[random.randint(0, len(string.ascii_letters)-1)] for _ in range(random.randint(1, 30))])
	k = ''.join([string.ascii_letters[random.randint(0, len(string.ascii_letters)-1)] for _ in range(len(p))])

	x = otp(p, k)

	return x, p, k

random.seed(int(time.time()))

print("Welcome to my one time pad service!\nIt's so unbreakable that *if* you do manage to decrypt my text, I'll give you a flag!")
print("You will be given the ciphertext and key for samples, and the ciphertext for when you try to decrypt. All will be given in base 64, but when you enter your answer, give it in ASCII.")
print("Enter:")
print("\t1) Request sample")
print("\t2) Try your luck at decrypting something!")

while True:
	choice = int(input("> "))
	if choice == 1:
		x, p, k = genSample()
		print(base64.b64encode(x.encode()).decode(), "with key", base64.b64encode(k.encode()).decode())

	elif choice == 2:
		x, p, k = genSample()
		print(base64.b64encode(x.encode()).decode())
		a = input("Your answer: ").strip()
		if a == p:
			print(os.environ.get("FLAG"))
			break

		else:
			print("Wrong! The correct answer was", p, "with key", k)
```

The source gives us this one-time padding system where each character of the string is Xor-ed with the corresponding character of the key. Its notable that both the plaintext and key have the same length.

Now normally for these types of questions, we give our user input to check the padding output or the question have us padded multiple things with the same key; hence letting us some idea about the key.
 
But here we have nothing of such sorts... though the plaintext generation seems interesting hmmm...

It takes a random integer between 1 and 30 to determine the length of the plaintext which is a ascii letter... so just maybe if we brute-force the system with just one ascii letter which has a possibility of happening, then we may have our flag...

```python
from pwn import *

host="misc.2020.chall.actf.co" 
port=20301

s=remote(host,port)

while 1:
	s.sendline('2')
	#print(s.recv())
	s.sendline('A') #trying with capital 'A' coz wynaut...
	answer=s.recv()
	if b'actf' in answer:
		print(answer)
		exit()

s.close()
```

It may take some time 2-5 minutes, yeah... but we get the answer, *actf{one_time_pad_more_like_i_dont_like_crypto-1982309}* 


# Confused Streaming

### Crypto

### Points: 100

### Description:
I made a [stream](https://files.actf.co/d3a7381b6dffcfdd427fd7f0e57221afcfbe8e4a3360dddfc55310b24cd4613b/chall.py) cipher!

*nc crypto.2020.chall.actf.co 20601*

## Solution:

This is the most funny challenge, I solved in a while. The code here,,,

```python
from __future__ import print_function
import random,os,sys,binascii
from decimal import *
try:
	input = raw_input
except:
	pass
getcontext().prec = 1000
def keystream(key):
	random.seed(int(os.environ["seed"]))
	e = random.randint(100,1000)
	while 1:
		d = random.randint(1,100)
		ret = Decimal('0.'+str(key ** e).split('.')[-1])
		for i in range(d):
			ret*=2
		yield int((ret//1)%2)
		e+=1
try:
	a = int(input("a: "))
	b = int(input("b: "))
	c = int(input("c: "))
	# remove those pesky imaginary numbers, rationals, zeroes, integers, big numbers, etc
	if b*b < 4*a*c or a==0 or b==0 or c==0 or Decimal(b*b-4*a*c).sqrt().to_integral_value()**2==b*b-4*a*c or abs(a)>1000 or abs(b)>1000 or abs(c)>1000:
		raise Exception()
	key = (Decimal(b*b-4*a*c).sqrt() - Decimal(b))/Decimal(a*2)
except:
	print("bad key")
else:
	flag = binascii.hexlify(os.environ["flag"].encode())
	flag = bin(int(flag,16))[2:].zfill(len(flag)*4)
	ret = ""
	k = keystream(key)
	for i in flag:
		ret += str(next(k)^int(i))
	print(ret)
```

It asks for us to enter 3 no.s *a,b and c* and then gives us conditions as whether to what their plausible values must be. If the values we enter satisfy the condition... then we get the flag.

Now one may carefully dive into the code and make a script, but why do when its clean infront of us. The relations between a,b and c clearly scream the quadratic equation; *ax^2+bx+c=0* and the comments in the code *# remove those pesky imaginary numbers, rationals, zeroes, integers, big numbers, etc*, tells us what the solution must me.

So, no imaginary no.s, zeroes, rationals, integers etc that leaves us only with *IRRATIONAL NUMBERS*. You dont even need a code for this... clearly you have studied enough quadratic equations in high-school to make a equation that has irrational roots. No? Simple man, take `b` to be larger than `a` and `c` and you are done 95% of times... heh? Here I am giving few examples but you may use any. a=1,b=5,c=1;a=2,b=13,c=3... sigh its so easy you will figure it out easily.

Once you give away those values to the server,

![LOL](Snips/ANGS/CONFUSED.PNG)

you get a binary string now use this simple code to get the flag!

```python
import binascii
a='01100001011000110111010001100110011110110110010001101111011101110110111001011111011101000110111101011111011101000110100001100101010111110110010001100101011000110110100101101101011000010110110001111101'
flag=binascii.unhexlify(hex(int(a,2))[2:])
print(flag)
```

You get this sweet answer, *actf{down_to_the_decimal}*


# Revving Up

### Reverse

### Points: 50

### Descriprion:
Clam wrote a [program](https://files.actf.co/e9eef8d8736ec555b0e4bfab4473a2027be7a08cc688f1dd07dafb8eb42ead90/revving_up) for his school's cybersecurity club's first rev lecture! Can you get it to give you the flag? You can find it at /problems/2020/revving_up on the shell server, which you can access via the "shell" link at the top of the site.

## Solution:

The program provided is a binary file... using strings in it we see this chunk...

![rev](Snips/ANGS/REV1.PNG)

So we need to just add *banana* as args while running the file and the program should throw us the flag.

Trying this now in the shell,

![revvv](Snips/ANGS/REV.PNG)

*actf{g3tting_4_h4ng_0f_l1nux_4nd_b4sh}* is the flag text!


# Windows of Opportunity

### Reverse

### Points: 50

### Description:
Clam's a windows elitist and he just can't stand seeing all of these linux challenges! So, he decided to step in and create his own [rev challenge](https://files.actf.co/240350c93b77621aaca286cac8c01b70be3aab4acbe9355f7f141716d0a6920e/windows_of_opportunity.exe) with the "superior" operating system.

## Solution:
 
It was easy as day... just use strings and grep actf on the binary file and you will get the flag

>strings windows_of_opportuniy.exe \| grep actf{.\*}

The flag comes as *actf{ok4y_m4yb3_linux_is_s7ill_b3tt3r}*


#Inputter

### Misc

### Points: 100

### Description:
Clam really likes challenging himself. When he learned about all these weird unprintable ASCII characters he just HAD to put it in a [challenge](https://files.actf.co/1d1b7b98e37d42fd514dda0a3aeac13da064206b51b07bd6366c8f469cf29736/inputter). Can you satisfy his knack for strange and hard-to-input characters? Source.

Find it on the shell server at */problems/2020/inputter/.*

## Solutions:

It was simple but mind-boggling. My C skills are weak as of now... Hoping to improve them.

Here, all you need to so is use printf func to pipe those unprintable charcater to the program and you are good to go.

>printf '\x00\x01\x02\x03\n' \| ./inputter $' \n\'\"\x07'

This gives us the flag, *actf{impr4ctic4l_pr0blems_c4ll_f0r_impr4ctic4l_s0lutions}*


# clam clam clam

### Misc

### Points: 70

### Description:
clam clam clam clam clam clam clam clam clam *nc misc.2020.chall.actf.co 20204* clam clam clam clam clam clam

>Hint: U+000D

## Solution:

The netcat server spams us some infinitely lines of clams and malcs... in what looks like flag format... 

Ehh? We should analyse whatever is being send by piping it to a file. So,

>*nc misc.2020.chall.actf.co 20204 > data.txt*

The server goes on infinitely so using ctrl+c and then analysing the file, we get lots of clams amd malcs and some random bits of this message...

![clam](Snips/ANGS/CLAM.PNG)

Huh, so we need to type 'clamclam' for salvation... lets try echoing that to the server...

>*echo "clamclam" \| nc misc.2020.chall.actf.co 20204*

Woo! we are instantly returned with the flag... *actf{cl4m_is_my_f4v0rite_ctfer_in_th3_w0rld}*