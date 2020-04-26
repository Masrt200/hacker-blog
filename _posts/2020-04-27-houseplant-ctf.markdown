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
