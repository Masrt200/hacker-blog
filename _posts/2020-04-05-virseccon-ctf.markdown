---
title: VIRSECCON CTF
layout: post
---

![Banner](Snips/VSCCTF/BANNER.PNG)


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
