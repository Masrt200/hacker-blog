---
title: zh3r0-CTF (Pre-CTF)
layout: post
---

![BANNER](Snips/zh3r0/BANNER.jpg)


# is this the robo loanguage?

### Crypto

### Points:495

### Description:
I have found some thing can you tell me what's inside it.

## Solution:

We are give a file which contains this:

```
--.- .-- -..- ... -.-- ... -... -- .- .-- ..... .-.. .. --. .-- -. .- --. ..-. .--. .. .... -. .--- -... ...-- -... .-.. -.-. -. .-. ...-


3,4,8,9,12,15,16,17,20,24,25,28,29,30,32
```

Decoding the morse code gives us a base64-like string! 

`QWXSYSBMAW5LIGWNAGFPIHNJB3BLCNRV`

But decoding this, gives us junk! Now if we take the numbers given and play with the the character in those index we might get something!

We actually had to turn the letters at those indices to lowercase

```python
a='QWXSYSBMAW5LIGWNAGFPIHNJB3BLCNRV'
b=[3,4,8,9,12,15,16,17,20,24,25,28,29,30,32]
c=[a[i].lower() if i+1 in b else a[i] for i in range(len(a))]
print(''.join(c))
```

We now have the correct base64... decoding it gives this `Alla fine l'hai scoperto`, which seems like another language but lets try translating it!

yoo! its translates to `In the end you found out`

Hence the flag is *zh3r0{In_the_end_you_found_out}*


# AES

### Crypto

### Points: 567

### Description:
You people kept asking about AES. So here it is

0d152c0ffefd78da21f04c769546b29b

enc(Key)="dnmjrqsqfsfnfqgt"

>Hint: no 13 may help you

## Solution:

I'm mad!!! was sleeping after having enough of brainfuck in general, when they released the hint for this one!

Anyway, it's AES decryption using ECB mode since we don't have any `IV` here. As the challenge depicts the key in encoded! ... well, 13! ROT13!

so `ROT13(key)==qazwedfdsfsasdtg` ... and you don't need my help but still!

```python
from Crypto.Cipher import AES

c='0d152c0ffefd78da21f04c769546b29b'
Key=b"qazwedfdsfsasdtg"

cipher=AES.new(Key,AES.MODE_ECB)
a=cipher.decrypt(bytes.fromhex(c))
print(a)
```

You will get *zh3r0{AES_1s_C00000l}* as flag!


# Ultra Strong RSA

### Crypto

### Points: 920

### Description:
A simple RSA maybe be easy to crack, what if we use multilayered encryption, where we encrypt the flag in multiple methods. To crack an advanced RSA Script, study the script well before even trying to crack it.

Not always tools help.

HAPPY HACKING :)

## Solution:

We are given a python script and a textfile...which has RSA encrypted text in it!

```python
from base64 import b64encode as b64e
from Crypto.Util.number import *
from Crypto.PublicKey import RSA
from flag import flag, hint

pt=bytes_to_long(flag)
ht=bytes_to_long(hint)
p=getPrime(2048)
q=getPrime(2048)
r=getPrime(2048)
s=getPrime(2048)
x=getPrime(1024)
y=getPrime(1024)
e=65537
if x>y:
	key=x-y
else:
	key=y-x

key=b64e(str(key))

n=x*y
n1=p*q
n2=r*s
n3=n1*n2

if p>q:
	a=p-q
else:
	a=q-p
b=r+q
c=p+r

enh=pow(ht,e,n)
ct=pow(pt,e,n3)

opt="N : "+str(n)+"\ne : "+str(e)+"\nenc(Hint) : "+str(enh)+"\n\nN1 : "+str(n1)+"\nN2 : "+str(n2)+"\nN3 : "+str(n3)+"\nCipher Text : "+str(ct)+"\n\n\nkey : "+str(key)+"\n"
pvt="a = "+str(a)+"\nb = "+str(b)+"\nc = "+str(c)+"\n"
test="p="+str(p)+"\nq="+str(q)+"\nr="+str(r)+"\ns="+str(s)+"\nx="+str(x)+"\ny="+str(y)+"\n"
#print(opt,pvt)


ob=open("out.txt","w")
ob.write(opt)
ob.close()
ob=open("hint.txt","w")
ob.write(pvt)
ob.close()
ob=open("test.txt","w")
ob.write(test)
ob.close()
```

At first the hint which is important is encrypted with `(e,n)`. Info about `n` is in `key` which is `x-y`or`y-x`! Simple qudractic here... `x\*\*2+key\*x-n=0` will give us x,y!

```python
from Crypto.Util.number import inverse,long_to_bytes
from sympy import solve,Symbol

hint_cp=<REDACTED>
N1=<REDACTED>
N2=<REDACTED>
N3=<REDACTED>
N=<REDACTED>
KEY=<REDACTED>
C=<REDACTED>
e=65537

y=Symbol('y')
Y=solve(y**2+KEY*y-N,'y')[1]
X=N//Y

tot_h=(X-1)*(Y-1)
d_h=inverse(e,tot_h)

hint=long_to_bytes(pow(hint_cp,int(d_h),N))
#print(hint)
#hint:https://pastebin.com/Ss2RBhVN
```

We get a link, and the data for the next part which is similar to the first one! Quadratics here too!


```python
a = <REDACTED>
b = <REDACTED>
c = <REDACTED>
#a=q-p,b=r+q,c=r+p

q=Symbol('q')
P=solve(q**2+a*q-N1,'q')[1]

Q=a+P
R=c-P

S=N2//R

tot=(P-1)*(Q-1)*(R-1)*(S-1)

d=inverse(e,tot)

m=pow(C,int(d),N3)

print(long_to_bytes(m))
```

Finally you get *zh3r0{Y0u_h4v3_b34ten_RSA}* !!!


# BaseXOR

### Crypto

### Points: 481

### Description:
The author seemed to find a strange string can u help him in decoding them

FQ0ACgAUVgcCSTAdAwpvGA0AFm8EVkonAVo6BkhCWBg=

## Solution:

Decode the base64, you get this!

`b"\x15\r\x00\n\x00\x14V\x07\x02I0\x1d\x03\no\x18\r\x00\x16o\x04VJ'\x01Z:\x06HBX\x18"`

Now lets XOR the first few bytes with the flag-format `zh3r0`

We get `oe3x0`... this might possible be the key we need! Let's XOR it with the whole thing!

Yoo! We get *zh3r0{34zy_x0r_wh3n_k3y_15_50r7}*!! Our flag!


# Are you the Master? 2

### Master

### Points: 983

### Description:
Enter key as the flag. (part1) folder in drive may help you

## Solution:

We get another RSA encrypted file with the source code! It's similar to the Ultra RSA chall but yet different! See here, we have a maths problem which can mislead us to may places!

So, the important info given is:

**N1=p\*q
N2=r\*s
A=p+q+r+s
B=(p-q)-(r-s)**

Now after sneding few hours trying bi-quadractics, getting __iotas__ and skimming through my old high-school notes, I got it!

![master0](Snips/zh3r0/master1.png)

See, this way we get a quadratci in `sq` which fun fact... also gives `pr` as its other root! So, when we get `pr` and `sq`, we can perform __GCD__ on it and obtain *p,q,r and s*!!

```python
from sympy import Symbol,solve
from Crypto.Util.number import *

N1=<REDACTED>
N2=<REDACTED>
E =65537
ct1 =<REDACTED>
ct2 =<REDACTED>

#p+q+r+s
A=<REDACTED>

#(p-q)-(r-s)
B=<REDACTED>

#t1t2=A+B+2-C

x_coeff=(A**2-B**2)//4-(N1+N2)


x=Symbol('x')
sol=solve(x**2-x_coeff*x+N1*N2,'x')

X,Y=sol[0],sol[1]

p=GCD(X,A)
q=GCD(Y,A)

tot1=(p-1)*(q-1)
d1=inverse(E,tot1)

m=pow(ct1,int(d1),A)
print(long_to_bytes(m))
```

*zh3r0{Th1s_1s_4_K3y_gfh}* the flag or a key?


# Are you the Master? 3

### Crypto

### Points: 1199

### Description:
You must me the master to get till here.

now give me the flag.

drive part 2 folder.

## Solution:

In the drive folder we get a binary name `iv` and a `img.txt`. Now *iv* can only mean the initialization vector of an AES encrypted data!

The flag from *Master_3* says its a key... so maybe thats all we need! Running the IV tho... prints the message It's a string, so maybe we have to do strings on it to get the iv!

![master3](Snips/zh3r0/master3.png)

The `H` character her seems unecessary so let's remove it! We get `Here is you IV: ryq9rkoU+XhtIuoFAvujDQ==`

Its seems we are ready to decrypt!!!

```python
from Crypto.Cipher import AES
from base64 import b64decode

iv=b64decode('ryq9rkoU+XhtIuoFAvujDQ==')
key='zh3r0{Th1s_1s_4_K3y_gfh}'

dat=b64decode(open('img.txt','rb').read())

cipher=AES.new(key,AES.MODE_CBC,iv)

plaintext=b64decode(cipher.decrypt(dat))
#print(plaintext)
f=open('flag.png','wb').write(plaintext)
```

In the plaintext, we see corrupted PNG headers and also other corruptions. Lets fix those with ghex!

![ghex](Snips/zh3r0/master32.png)

Now we see the flag on opneing the file!

![flag](Snips/zh3r0/master33.png)

*zh3r0{You_are_master_Of_All_Arts}* Cheers!


