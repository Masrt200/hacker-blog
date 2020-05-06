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

It we actually had to turn the letters at those indices to lowercase

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


