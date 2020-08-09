---
title: spiderlabs-ctf
layout: post
---

![Banner](Snips/spiderlabs/banner.png)

# Software Defined Radio! 

### Introduction

This category makes use of skillset of reversing-engineeing captured signals!

I made use of [Inspectrum](https://github.com/miek/inspectrum), as the challenge said to decode the signal, but it can also be done in many other ways!

Let's check the first challenge as tutorial for the last two!

### You turn me on and off [150]

#### Description:
We captured alien radio data... Can you crack their message?
[sdr-challenge1.cfile](http://spiderlabsctf.com/sdr-challenge1.cfile)

> p.s. download inspectrum

So, open after the hassle of setting up a new software and not messing up your system, open the challenge file with inspectrum!

`inspectrum sdr-challenge1.cfile`

Scroll the cursor, till you get to the captured packects!

![packtes](Snips/spiderlabs/packtes.png)

Leave the settings as default, the main purpose here is to make the signal "*noise free*", and clear, so we can adjust the settings to do that!

But as it seems the default settings suits the best!

![defualt](Snips/spiderlabs/defaults.png)

Now, in the `Time Selection Section`, set Symbols to 25 or so and enable the cursors! And then move and resize the region that appears so that the *smallest* region aligns itself to the length of the *smallest* signal block!

![ini_align](Snips/spiderlabs/initial_alignment.png)

Somewhat like this closely!

![pixel](Snips/spiderlabs/pixeli.png)

Then increase the no of Symbols and realign them as per the blocks, do this till the entire signal data is covered with the cursors!

![alignment](Snips/spiderlabs/alignment.png)

> Now its easy peasy from here!

Right-click the signal-data and then do `Add Derived Plot > Amplitude Plot`!

A red horizontal line will appear!

![amplitude](Snips/spiderlabs/amplitude.png)

Just drag the red line, over the top of the signal data till you see a nice amplitude graph, *niceness depends upon your alignment*

![amplitude_plot](Snips/spiderlabs/amplitude_plot.png)

Now, you maybe we wondering why we are doing all this! See, our main motive is to extract the binary data encoded withing the signal! the smallest signal block represents a `1` and a blank space represents a `0` ... the initial bits go like this `1000110` which decode to ascii-`F` , get it?

Now, you are free to extract bits manually but this way is just better! *Although I did extract it manually for the first two challs until I discovered this method! :(*

So finally, right-click on the plot and do `Extract Symbols > To stdout`

![extraction](Snips/spiderlabs/extract.png)

Now open the terminal, and you will the extracted bits although not accurate its all that we need!

![stdout](Snips/spiderlabs/stdout.png)

Now fire up python and enjoy!

```python
>>> c='0.296352, -0.999998, -0.999996, -0.999996, 0.322055, 0.312156, -0.999999, -0.999981, 0.29718, 0.295499, -0.999999, 0.308109, 0.321133, -0.999996, -0.999997, -0.999993, 0.301978, 0.316911, -0.999998, -0.99999, -1, -1, 0.298925, -0.999997, 0.304455, 0.318662, -0.999992, -0.999991, 0.305682, 0.327311, 0.316653, -0.999996, -0.999993, 0.309524, -0.999999, 0.310872, 0.30265, -0.999999, 0.314892, -0.999997, 0.317225, 0.316971, -0.999999, -0.999998, 0.315871, 0.321062, -0.999992, -0.999999, -0.999999, 0.318565, 0.322979, -0.999996, -0.999996, -0.999985, -0.999992, -0.999998, 0.293918, 0.314347, -0.999996, -0.999999, 0.304823, 0.294362, -0.999994, -0.999998, -1, 0.284555, 0.315224, -0.999997, 0.280219, -0.999999, -0.999999, -0.999996, -1, 0.283892, 0.306363, -0.999995, 0.302248, 0.302009, 0.303467, -0.999994, 0.321145, 0.296669, -0.999994, -0.999993, 0.308316, 0.307696, -0.999999, -0.999985, 0.307899, 0.288028, -0.999993, -0.999998, -0.999999, -0.999994, 0.308951, -0.999984, -0.999998, 0.304631, 0.291852, -0.999991, -0.999983, 0.282598, 0.319025, -1, -0.999994, 0.306636, 0.296733, -0.999999, -0.999995, 0.295223, -0.999998, -0.999999, 0.298153, 0.309762, -0.999996, -0.999998, -0.999996, 0.292907, 0.305967, -0.999993, 0.295379, 0.291825, -0.999996, -0.999997, 0.283395, -0.999996, 0.297688, -0.999993, 0.305137, 0.303011, -0.999984, -0.999998, 0.288985, -0.999986, 0.289759, -0.999993, -0.999995, 0.283813, 0.293202, 0.293763, -0.999996, -0.999992, 0.295041, -0.999999, -0.999996, 0.289967, 0.282267, -0.999994, -0.999997, 0.28599, 0.312664, -0.999992, 0.29214, 0.300696, -0.999993, -0.999998, 0.301849, -0.999997, 0.307317, -0.999999, -0.999983, 0.295189, 0.298169, -0.999998, 0.303839, 0.293838, -0.999999, -0.999991, 0.297174, 0.281549, -0.999981, -0.999998, -0.999999, 0.304161, 0.29304, -0.999989, -0.999996, 0.284885, 0.293954, -0.999996, -0.999993, 0.29524, 0.282804, -0.999993, -1, 0.29767, 0.284739, -1, -0.999984, 0.291954, -1, -0.999985, 0.295192, 0.288656, -0.999986, -1, -1, 0.277962, 0.286773, -0.999995, -0.999995, 0.29657, 0.281652, 0.284367, -1, -0.999999, 0.272971, -1, -0.999996, 0.301735, 0.276792, -1, -0.999993, 0.271849, 0.27387, -1, 0.273795, 0.260515, -0.999998, -1, 0.276843, -0.999998, 0.261844, -0.999998, 0.270989, 0.267367, -0.999998, -1, 0.276232, -1, 0.268199, -0.999999, -0.999993, 0.273989, 0.279627, -0.999999, 0.268841, 0.2762, -0.999998, -0.999994, -0.999985, 0.286941, 0.270063, -1, 0.274396, -0.999992, -0.999997, -0.999984, -0.999993, 0.283191, 0.275578, 0.28392, -0.999996, -0.999996, -0.999994, -0.999995, 0.288057, 0.302771, -0.999999, -0.999991, -0.999997, 0.294791, 0.300922, -0.999997, -0.999992, 0.313433, 0.320796, -0.999997, -0.999998, -0.999999, 0.304164, -1, -0.999997, 0.314363, 0.319958, 0.315523, -0.999996, -0.999999, 0.295627, -0.999996, 0.329332, 0.318756, -1, -0.999998, 0.313605, -0.999997, -0.999995, -0.999997, 0.315632, 0.314954, -0.999988, -1, -1, 0.29864, 0.32669, 0.30328, -1, -0.999999, -0.999999, -0.999997'
>>> c=c.split(', ')
>>> b=''
>>> for i in c:
		b+=str(round(float(i))+1)
>>> b
'100011001101100011000010110011100101101011001100011000001100110001101000011011101100110011000010011001100110010011000110110010101100101001110010011001101100101001101100110001100110011001100100110001100111001001100110110010101100101001101100011010000111000011000110011000100111001011001000110001110000'

#you can decode this using cyberchef or modify it in python! prepend a zero to the result; 
#as first byte has only 7 bits!

#or play with python
>>> bytes.fromhex(hex(int('0'+b[:-5],2))[2:])
b'Flag-f0f47fa32cee93e6c32c93ee648c19dc'
```