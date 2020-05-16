---
title: Noob_CTF (Pre)
layout: post
---

![BANNER](Snips/noob0x1/BANNER.png)

# Fake IP

### Web

### Points: 100

### Description:
Elliot found that someone might have messed up with challenge creation. Check, if they provided the right IP for this challenge.

[site](https://chall.noobarmy.tech/Fake_ip/)

## Solution:

The link takes you a site... let's check the source code.

![js](Snips/noob0x1/fake1.png)

On the last line we have this `assets/js/main.js` source link! Clicking it and reaching the end of that page, we see a comment!

```
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>++.-----.++++++++++.------.<<++.>>+.++++++.-----------.++++++.++++++++++++.<<.>>------------------.+++++++++++++++++.-------------.<<.>>++++++++++.+++++++.-----------------.+++++++++++++.+++.---------.-------.-.<<.>>++++++++++.+.++++++++.<<++++++++++++++.--------------.>+++++++++++++++++.++++++++++++++.+++++++.---------.>--------.--.<++.<.>>+++++++.-----.<<.>>+++++.<+++.---.<.>>+++.--------.+++.------.<-.<.>>+++.<++.<.>.-----.>----.<++++.<.>----------------------------.+++++++.<+++++++.>>++++++++.<<+++++++.--------------.>>-----------.++++++++++++..----.+++.<----------------------.-----------..>----------------.+++++.-------.+++++++++++..<-.>++.+..-------------.-.+++++++++++++++++.-----.++++++++++++.<.>-----.---------------.--.+++++.<+.++.-.++.--.+++.---.-.
```

Now this is clearly `Brainfuck` language. Decoding it in [tio.run](https://tio.run/) we get,

`fake flags are overuled now. Welcome to the world of fake IP's. https://chall.noobarmy.tech/102030/`

Great! So we now have the correct address! Going to this site and veiwing the source code we see three comments as:

```
part1 - 5BjMEvLjTs
part 2- e2Y0a2VfZmw0Z3NfYXIz
part 3- ?ZRo.D)#R3CboF.Cc$3SI/
```

These seem like base64-ish encoded stuff... At first I concatenated them and used Base85 etc but it wasn't. Decode them seperately!

Part1 is Base58, 2 is Base64 and the 3rd part is Baase85 encoded. Decoding them we get the final flag as *noobctf{f4ke_fl4gs_ar3_s0_m1ddl3_cl4ss}*

# Bottled Something(?)

### Misc

### Points: 300

### Description:
Mr Holmes had a suspicion on Moriarty for the creation and spread of COVID-19, So when he started inspecting Moriarty's mails, He found that Moriarty sent this txt file to one of his closest friend. Maybe there's a vaccine in this thing? Take a look.

Sometimes opening files in browser helps. ;)

>Hint:There's a link on this page that'll be helpful.

[Page](https://www.github.com/Holmes-py/King-of-the-Hill)

Flag format: noobctf{flag_here}

## Solution:

Get the file and opening it you will see a large base64 encoded string... Now we don't exactly need to visit that page but a hint is what we get. In the last section of that page we see a challenge were a zipfile was created from as base64 encoded string.

Don't use that website linked use this one-liner to do it!

`cat Study_Encodings_dude.txt |  base64 -d > abc.zip`

Unzip the file and you will see loads of PNG images! At first I tried loads of other things with these but then I tried the other hint of opening them in the browser!

Now as you keep viewing each of them, you will come across 3 such images which are not infact PNGs but *'Animated PNGs'*... namely,

```
220x-sherlockwiki_studypink.png
239x-name-sherlock.png
300x-staricase.png
```

These *"Animated PNGs"* have a frame or 2 in which a **QR CODE** shows up. Scan them by whatever means you can. I myself wasted few screenshots to capture the frame and it was done that way!

*noobctf{Ah_Animated_PNGs_are_kinda_fun}* is the vaccine?!


# An0nym0us G1rl_fri3nd

### Forensics

### Points: 200

### Description:
r3curs1v3_pr0xy friends believes that his girlfriend is cheating on him.Somehow he managed to get a document from his girlfriend computer. But he found that it is encrypted. So he asked me to decrypt the file and get the data for him.Please help me to get the document decrypted.

>Hint:Common sense may help you.
>Hint:Search for some common password on internet. It may help you somewhere.

## Solution:

This was actually fairly easy! The docx file given is encrypted. We use,

`office2john.pl Office.docx > hash.txt`

To get the hash of the file. Now using simply `john hash.txt` gives us the passwor as `Disney`. This CTF had easy passwords. Now here where I hit the road block... Its kinda funny, I use Google Docx and My MS OFFICE is not activated for fuck's sake they don't let you copy the contents of the file. I used `abiword` simple but fine!

So yeah... open the file and you willa `Love Story` and lots of `spaces` and `tabs` in the lower sections. Hmm... must be `stegsnow`. Copy the contents of the docx to a text file and try stegsnow with `-C`, it doesn't work!

A while later the second hint was released i.e, to use common passwords. So use `password`, `password123` etc and you will find `12345678` works. *p.s. I bruteforced it!*

`stegsnow -p 12345678 -C snow.txt`

*noob{r3curs1v3_pr0xy_adm1r3_y0ur_kn0wl3dg3}* Thanks!


# Television Transmission

### Forensics

### Points: 300

### Description:
Google "Slow Scan Television Transmission". You will learn something new for sure.

>Hint:Do you know about LSB? Remember don't get trolled :)

## Solution:

Another wierd one huh? Mr. Proxy! Well, as the title and everything says we ought to use [SSTV](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=11&cad=rja&uact=8&ved=2ahUKEwipqq-KjLjpAhUJwzgGHeTkAEkQFjAKegQIAxAB&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSlow-scan_television&usg=AOvVaw3nYA14yEsOmzfwLWYjfL-g) for this. Going that route we get trolled by a smiley face laughing at us... so what to do?

The hint says lsb... set lets look for a lsb-tool on `.wav` files. We find [stegolsb](https://pypi.org/project/stego-lsb/)!

Using this tool was bit of a hassle but I got it!

`stegolsb wavsteg -r -i audio.wav -o output.txt -n 1 -b 100`

The `wavsteg` is used for .wav file steganography! Running it you get the flag,
*noob{lsb_c4n_h4v3_s0m3_ju1cy_d4t4}*. I leant...


# Web Steg

### Misc

### Points: 300

### Description:
Combination of web and stegnography is amazing.You have to think out of the box.

[link](https://chall.noobarmy.tech/web_steg/)

>Hint:Check the deliver to page.

## Solution:

This chall had the worst name ever, as discussed with the author! We are given a link to site whose source code doesn't gives much!

Initially I thought I had to do something with those the various image link available but it was a dead-end. Then one of the `.css` files had a image link but it was useless as well.

A lot dicussion with the author later and slamming my head a lot, the `dform.html` link had something to do with it!

![BANNER](Snips/noob0x1/websteg.png)

As you can see the are a no. of whitespaces or tabspaces even after where the text ends. This means only one thing... **stegsnow**. You seem to like it a lot author!

copy the text in the source-code of `dform.html` and put it into a text file! Use,

`stegsnow -C troll`

*noob{w3b_and_f0r3ns1c_c0mb1n4t10n}*. I have my doubts **400%!**