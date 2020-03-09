# RSA Beginner

### Points:90

### Description:
I found this scribbled on a piece of paper. Can you make sense of it? [link](https://mega.nz/#!zD4wDYiC!iLB3pMJElgWZy6Bv97FF8SJz1KEk9lWsgBSw62mtxQg)

## Solution:

Well, we have this file: *rsa(1).txt*, from the link.

![ls](https://github.com/Masrt200/hacker-blog/blob/gh-pages/Snips/CTFLEARN/CRYPTO/RSABEGINNER1.PNG)

While the description doesn't hint us much. The filename clearly suggests that its a RSA algorithm decryption challenge. And so does its contents...

![cat](https://github.com/Masrt200/hacker-blog/blob/gh-pages/Snips/CTFLEARN/CRYPTO/RSABEGINNER2.PNG)

If you like maths and its your first time solving RSA, trust me this [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) will have you in tour, scratching the basics for some good weeks. OR you can simply use this tool I made [D3cryptor](https://github.com/Masrt200/WoC2k19).

Once you have gone through the installation, you know that we need to adjust the file format for my tool to accept inputs. Using *nano* here,

![nano](https://github.com/Masrt200/hacker-blog/blob/gh-pages/Snips/CTFLEARN/CRYPTO/RSABEGINNER3.PNG)

Now simply giving the file to my tool D3crypt0r for decrypting this RSA;
```bash
cryptologer -d -R -f 'rsa (1).txt'
```

![RSA](https://github.com/Masrt200/hacker-blog/blob/gh-pages/Snips/CTFLEARN/CRYPTO/RSABEGINNER4.PNG)

Well there you have it the flag is *abctf{rs4_is_aw3s0m3}* Cheers!



