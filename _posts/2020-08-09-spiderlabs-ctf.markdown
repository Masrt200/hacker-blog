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

`$ inspectrum sdr-challenge1.cfile`

Scroll the cursor, till you get to the captured packects!

![packtes](Snips/spiderlabs/packets.png)

Leave the settings as default, the main purpose here is to make the signal "*noise free*", and clear, so we can adjust the settings to do that!

But as it seems the default settings suits the best!

![defualt](Snips/spiderlabs/defaults.png)

Now, in the `Time Selection Section`, set Symbols to 25 or so and enable the cursors! And then move and resize the region that appears so that the *smallest* region aligns itself to the length of the *smallest* signal block!

![ini_align](Snips/spiderlabs/initial_alignment.png)

Somewhat like this closely!

![pixel](Snips/spiderlabs/pixeli.png)

Then *increase* the no of Symbols and **re-align** them as per the blocks, do this till the entire signal data is covered with the cursors!

![alignment](Snips/spiderlabs/alignment.png)

> Now its easy peasy from here!

Right-click the signal-data and then do `Add Derived Plot > Amplitude Plot`!

A red horizontal line will appear!

![amplitude](Snips/spiderlabs/amplitude.png)

Just drag the red line, over the top of the signal data till you see a nice amplitude graph, *niceness depends upon your alignment*

![amplitude_plot](Snips/spiderlabs/amplitude_plot.png)

Now, you maybe we wondering why we are doing all this! See, our main motive is to extract the binary data encoded within the signal! the smallest signal block represents a `1` and a blank space represents a `0` ... the initial bits go like this `1000110` which decode to ascii-`F` , get it?

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

#you can decode this using cyberchef or modify it in python! prepend a zero to final result; 
#as first byte has only 7 bits! there are also so trailing zeros, which we can remove!

#or play with python
>>> bytes.fromhex(hex(int('0'+b[:-5],2))[2:])
b'Flag-f0f47fa32cee93e6c32c93ee648c19dc'
```

Now lets fly through the others shall we?

### Let's Get Scwhifty! [300]

#### Description:
The aliens are on to us... They've started wearing tin foil hats and switched things up with their radio transmission. Can you crack their latest message?

[sdr-challenge2.cfile](http://spiderlabsctf.com/sdr-challenge2.cfile)

> authors have a phd in loopholes and blackholes 

Things are a bit different here... you will notice that its not the previous plot this time, but rather a continous plot!

Increase the `FFT size` and `Zoom` by 3 notches to see things more clearly!

![plot2](Snips/spiderlabs/plot2.png)

Now lets experiment a different method here, since the blocks are a bit small to perfect align with the cursors, go to `Add Derived Plot > Add Frequency plot`!

Then right-click the plot and do `Add Derived Plot > Add Threshold plot`!!

![thres](Snips/spiderlabs/threshold.png)

You will see something like this!

![thres_plot](Snips/spiderlabs/thresholding.png)

Now you can easily align the cursor with the help of this!

![nicely_done](Snips/spiderlabs/cursors_thres.png)

We dont need an amplitude graph now, just right-click the threshold graph and do `Extract Symbols > To Stdout`

This time the output on the terminal will be exact bits! Cheers!

```python
>>> c='0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0'
>>> c=c.split(', ')
>>> b=''
>>> for i in c:
		b+=i
>>> b
'010001100110110001100001011001110010110101100101011001100011010101100001001100010011011001100100001100010110000100110000001100100110010101100011001100100011010101100110011000100011001000110100001100110110011000111001001100010110010001100101011001100011010001100110011000110011100100110011001101100010'

#some trailing bits again... cyberchef is better tbh, but lazy to do screencaps 

>>> bytes.fromhex(hex(int(b[:-4],2))[2:])
b'Flag-ef5a16d1a02ec25fb243f91def4fc936'
```

### Don't flip out, Thomas! [500]

#### Description:
The aliens' capabilities have advanced and they've decided to improve the reliability of their transmissions. Can you crack their message?

[sdr-challenge3.cfile](http://spiderlabsctf.com/sdr-challenge3.cfile)

> aliens are fools

This last one is extractly as some as the first one with some changes!

First you will notice that the signal is almost double the lenth, hmm!

![plot3](Snips/spiderlabs/plot3.png)

The graphs seems much nicer but the output is ridiculous! During the CTF, my output was near `-1's and 0's` but one half of the output this time was near `12's`, not a problem per say but WIERD!!

```python
>>> c='-0.99997, 11.1106, 11.1694, -0.999992, -0.99992, 11.0514, -0.999968, 11.2205, -0.999963, 11.2827, 11.3478, -0.999706, 11.2109, -0.999982, -0.999999, 11.3926, -0.999948, 11.4896, 11.3279, -0.999991, 11.3649, -0.999993, -0.999934, 11.4388, 11.4549, -0.999987, 11.5114, -0.999974, -0.999995, 11.457, -0.999936, 11.4815, -0.999993, 11.4435, 11.5223, -0.999986, 11.5477, -0.999972, -0.999988, 11.3776, -0.999974, 11.5101, -0.99999, 11.3224, -0.999983, 11.3388, 11.4577, -0.999906, -0.999991, 11.4539, 11.3973, -0.999994, 11.3508, -0.999942, -0.999912, 11.4042, -0.999982, 11.4069, 11.126, -0.999917, 11.39, -0.99999, 11.402, -0.999971, -0.999905, 11.3848, -0.999994, 11.4153, 11.4869, -0.999947, -0.999994, 11.3809, 11.407, -0.999992, 11.5127, -0.999999, -1, 11.2166, 11.4048, -0.999997, -0.999987, 11.3736, -0.999984, 11.4122, 11.4557, -0.999969, 11.3333, -0.999862, -0.999958, 11.2766, -0.99996, 11.442, 11.3356, -0.999952, 11.4744, -0.999998, -0.999985, 11.3108, 11.2808, -0.99999, 11.4343, -0.999953, -0.999961, 11.5343, -0.99996, 11.2761, -0.999791, 11.4361, 11.462, -0.999798, 11.4301, -0.999996, -0.999568, 11.4521, -0.999577, 11.3659, 11.3863, -0.999829, 11.2894, -0.999966, -0.99977, 11.2104, -0.999741, 11.34, 10.9381, -0.999562, 10.8276, -0.999992, -0.998843, 11.4343, 11.1713, -0.998019, 11.2633, -0.999994, -0.996435, 11.5627, -0.996624, 12.0348, -0.99681, 11.3207, 12.5427, -0.999923, -0.99685, 12.7359, -0.995529, 12.6011, -0.991811, 11.4391, 11.8511, -0.981237, 11.3679, -0.999999, -0.974221, 11.5376, 8.69522, -0.97395, 7.13133, -0.999956, -0.966855, 4.86812, -0.933531, 11.2898, 2.72975, -0.776728, 1.46008, -0.99995, -0.407322, 0.277943, 0.105821, 11.248, -0.459846, -0.999998, 1.42713, 11.4811, -0.84345, -0.99995, 3.63274, 11.4124, -0.936897, 5.25712, -0.940375, -0.999943, 6.75393, -0.944075, 7.80711, -0.951536, 8.42594, 11.6765, -0.965547, 8.55706, -0.963816, -0.999956, 9.526, 11.5646, -0.98561, 9.87272, -0.991985, -0.999885, 10.5751, -0.997044, 10.509, 11.4934, -0.999509, 10.9338, -0.999938, -0.999993, 11.3851, -0.999757, 11.2488, 11.6994, -0.999778, 11.7194, -0.99954, -0.999955, 11.6482, -0.999371, 11.7608, -0.999642, 11.8469, 11.3558, -0.999358, 11.7235, -0.999738, -0.999985, 11.7062, -0.999893, 11.5553, 11.4792, -0.999949, 11.7136, -0.999989, 11.3926, -0.999937, -0.999962, 11.3578, -0.99995, 11.512, -0.999984, 11.2242, -0.999968, 11.3714, -0.999968, 11.1957, 11.3752, -0.999876, 11.3421, -0.999881, -0.999984, 11.4418, -0.999987, 11.3295, -0.999989, 11.3639, -0.999995, 11.1448, -1, 11.2525, -0.999947, 11.4662, 11.4968, -0.999956, 11.2253, -0.999972, 11.2004, -0.999967, -0.999922, 11.3457, -0.999953, 11.3532, -0.999999, 11.3621, -0.99998, 11.3495, -0.999971, 11.402, 11.4859, -0.999956, 11.28, -0.999987, -0.999999, 11.3755, 11.5724, -0.99999, -0.999987, 11.5551, 11.5318, -0.999985, -0.999998, 11.5093, -0.999975, 11.4682, 11.3485, -0.99994, 11.442, -0.999937, 11.4189, -0.999951, -0.999969, 11.4334, -0.999974, 11.3993, -0.999999, 11.4988, -0.999929, 11.5455, -0.999947, 11.467, 11.5224, -0.999968, 11.5196, -0.999897, 11.3603, -0.999869, -1, 11.476, -0.999996, 11.7765, -0.999954, 11.4227, -0.999948, 11.6779, 11.5175, -0.999988, 11.5862, -0.999993, -0.999983, 11.3233, -0.999983, 11.3699, -0.999747, 11.4706, 11.53, -0.999929, 11.501, -0.999981, -1, 11.4644, -0.999996, 11.4545, 11.5542, -0.999924, 11.5259, -0.999954, -0.999991, 11.4125, -0.99999, 11.4324, 11.4998, -0.999974, 11.3682, -1, -0.999994, 11.3739, -0.999999, 11.5215, 11.5162, -0.999993, 11.3841, -0.999984, -0.999991, 11.4099, 11.701, -0.999991, -0.999904, 11.417, 11.5382, -0.999938, -0.999831, 11.5091, -0.999947, 11.4452, 11.4956, -0.999997, 11.6209, -0.999965, -0.999908, 11.6406, 11.4881, -0.999985, -0.999937, 11.5793, 11.5048, -0.999999, -0.999977, 11.6667, -0.999864, 11.6195, 11.4243, -0.999984, 11.453, -0.99999, -0.999942, 11.9443, 11.7193, -0.99994, 11.5859, -0.999991, 11.8026, -0.99998, -0.999891, 11.5995, 11.9154, -0.999911, 11.8573, -1, -0.999973, 11.766, -0.999995, 11.8198, -0.999977, 11.8024, 11.7497, -0.999933, -0.999969, 11.9119, -0.999981, 11.8596, 11.8561, -0.999906, 11.7955, -0.999977, -0.999998, 11.8521, -0.999998, 12.1264, 12.0249, -0.999994, 11.9876, -0.999986, -0.999973, 12.0215, -0.999913, 12.0875, -0.999947, 12.1385, 11.8405, -0.999831, 11.9156, -0.999943, -0.999982, 11.965, -0.999893, 11.9324, 11.9148, -0.99995, 11.8908, -0.999996, -0.99998, 11.9732, -0.999998, 12.0759, 11.8144, -0.999946, 11.9711, -0.999971, -0.999971, 11.8938, -0.99994, 12.0442, -0.999996, 11.9205, 12.0126, -0.999986, -0.999998, 12.1517, 11.9, -0.999919, 11.8862, -0.999991, -0.99992, 11.889, -0.999941, 11.9077, 12.0352, -0.999969, -0.999997, 11.861, -0.999978, 11.9759, -0.999988, 11.9281, -0.999982, 11.869, 11.9888, -0.99996, 11.9993, -0.999988, -0.999932, 11.9825, -0.999939, 12.0927, -0.999998, 12.143, -0.999904, 11.9318, -0.999978, 12.001, -0.999992, 12.2496, 12.12, -0.999982, 12.2498, -0.999989, -0.999907, 12.0789, -0.999907, 12.1036, 12.1294, -0.999961, 12.188, -0.999997, -0.999947, 12.0596, -0.999857, 12.0133, 12.2547, -0.999989, 12.09, -0.999975, 11.9732, -0.99996, -0.999942, 12.1539, -0.999841, 12.1453, 11.9095, -0.999982, -0.999971, 12.2394, -0.999996, 12.2216, 11.9789, -0.999989, 12.1954, -0.999992, -0.999994, 12.1189, -0.99988, 12.271, 12.1448, -0.999989, -0.999974, 12.1851, -0.99999, 12.2271, -0.999991, 12.2379, 12.1345, -0.999996, 12.2838, -0.999985, -0.999987, 12.4144, 12.3354, -0.999957, 12.3025, -0.999971, 12.0337, -0.999995, -0.999999, 12.4081, -0.99999, 12.3376, 12.1878, -0.999874, 12.175, -0.999995, -0.999967, 12.3091, 12.3558, -0.999985, -0.999974, 12.3708, -0.999985, 12.1641, -0.999961, 12.3227, -0.999963, 12.4121, 12.2726, -0.999891, 12.4122, -0.999961, -0.999889, 12.2769, 12.2579, -0.99996, -0.999738, 12.4337, -0.999352, 12.1021, 12.2841, -0.99999, -0.999939, -0.999931, -0.999946, -0.999954, -0.999984, -0.999998, -0.999911'

>>> c=c.split(', ')
>>> b=''
>>> for i in c:
		a=round(float(i))
		if a==-1:
			b+='0'
		else:
			b+='1'

>>> b
'0011010100101101001011001011010100101101010100110010110100101011010011001011001101001011010010110010110101001011010010110100101100101101010011010100101100101101001011010011001100101101010010110010110100101101001011010100101101001010110101010100101101010101010010101101010101001011001100110100101011010101010010101101010100101101010010110100101101001011010010110011001101001011001100110100101100101011001011010100110100101101001011010100101101001011010010110101001100101101001101010100101101010101010010110100101101001010110100110100101101001101010010110010101101001011001101010100101100110100'
>>>len(b)  #removing extra bits
592
```

Now you will notice that this bit-stream is exactly twice the length of bit-stream of the FLAG which is 37 bytes, **37\*8\*2= 592**!

So, the bits must be encoded somehow! Its not that hard! By observing the bits of the first-byte of the flag, `F` i.e, `01000110` we can deduce that:

```
1 --> 10,11
0 --> 00,01
```

Let's end this shall we?

```python
>>> flag=''
>>> for i in range(0,592,2):
	m=b[i:i+2]
	if m=='01' or m=='00':
		flag+='0'
	else:
		flag+='1'

		
>>> bytes.fromhex(hex(int(flag,2))[2:])
b'Flag-3c3b6ecfc808588c3557bf31d0392744'
```

Thanks for reading!!

Related post and a pdf that could help!!

[Inspectrum_Reverse_with_DSpectrumGUI](https://raw.githubusercontent.com/tresacton/dspectrumgui/master/public/DSpectrumGUI%20%E2%80%93%20Reverse%20Engineering%20Guide.pdf)

[opensource-sdr-movement](https://medium.com/@nihal.pasham/rf-reverse-engineering-has-become-trivial-thanks-to-the-opensource-sdr-movement-d1f9216f2f04)