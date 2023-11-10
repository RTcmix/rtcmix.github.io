---
title: FILTSWEEP()
layout: ref
---

## FILTSWEEP

Time-varying biquad (bandpass) filter.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**FILTSWEEP**(outsk, insk, dur, AMP, ringdowndur, steepness, ampbalance,
inputchan, PAN, BYPASS, CENTERFREQENV, BANDWIDTHENV)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | ring-down duration | seconds | no | no | 
p5 | steepness | 1-5, inclusive | no | no | 1 is a good starting value | 
p6 | balance output and input signals | 0:no, 1:yes | no | no | usually use 1 | 
p7 | input channel |  -  | no | no | 
p8 | pan | 0-1 stereo; 0.5 is middle | yes | no | 
p9 | bypass filter | 0: bypass off, 1: bypass on | yes | no | usually use 0 | 
p10 | filter center frequency | Hz | yes | no | 
p11 | filter bandwidth | Hz if positive; if negative, the '-' sign acts as a flag to interpret the bw values as a multipler of the current center frequency (0-1) | yes | no |

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author: John Gibson, 2/5/00; rev for v4, JGG, 7/24/04

  

-----

  
**FILTSWEEP** filters the input sound using a series of time-varying
biquad filters. Center frequency and bandwidth can be dynamically
altered.

### Usage Notes

The ring-down duration parameter (p4) is to let you control how long the
filter will sound after the input has stopped. Too short a time, and the
sound may be cut off prematurely, resulting in a click.

p5 ("steepness") is just the number of filters to add in series. Using
more than 1 decreases the actual bandwidth of the total filter. This
sounds different from decreasing the bandwith of 1 filter using the
"BANDWIDTHENV" parameter (p11), described below. (Mainly, it further
attenuates sound outside the passband.) If you don't set p6
("ampbalance") to 1, you'll need to change p3 ("AMP") to adjust for loss
of power caused by connecting several filters in series. Guard your
ears\!

The "ampbalance" parameter (p6) tries to adjust the output of the filter
so that it has the same power as the input. This means there's less
fiddling around with p3 ("AMP") to get the right amplitude (audible, but
not ear-splitting) when the bandwidth is very low and/or sharpness is \>
1. However, it has drawbacks: it can introduce a click at the start of
the sound, it can cause the sound to pump up and down a bit, and it eats
extra CPU time.

The output of **FILTSWEEP** can be either mono or stereo.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("FILTSWEEP")
   
   rtinput("mysound.aif")
   
   inskip = 0
   dur = DUR() - inskip
   
   amp = 1.0
   balance = 0
   sharpness = 2
   ringdur = .5
   
   lowcf = 0
   highcf = 1600
   startbw = -.01
   stopbw = -1
   
   ampenv = maketable("line", 1000, 0,0, dur-.1,1, dur,0)
   
   cfenv = maketable("line", "nonorm", 1000, 0,lowcf, dur/3,highcf, dur,lowcf)
   bwenv = maketable("line", "nonorm", 1000, 0,startbw, dur,stopbw)
   
   FILTSWEEP(start=0, inskip, dur, amp*ampenv, ringdur, sharpness, balance, 0, 1, 0, cfenv, bwenv)
   FILTSWEEP(start=.1, inskip, dur, amp*ampenv, ringdur, sharpness, balance, 0, 0, 0, cfenv, bwenv)
```

  
  
another one:

```cpp
   rtsetparams(44100, 2)
   load("FILTSWEEP")
   
   rtinput("mysound.aif")

   inskip = 0
   dur = DUR() - inskip
   
   bypass = 0
   
   amp = 1.1
   env = maketable("line", 1000, 0,0, dur-.1,1, dur,0)
   
   ringdur = 0.5
   balance = 0
   steepness = 2
   
   lowcf = 90
   highcf = 4000
   narrowbw = -0.02
   widebw = -0.90
   
   cf = maketable("line", "nonorm", 2000, 0,lowcf, dur/3,highcf, dur,lowcf)
   bw = maketable("line", "nonorm", 2000, 0,narrowbw, dur,widebw)
   
   start = 0
   FILTSWEEP(start, inskip, dur, amp * env, ringdur, steepness, balance,
      inchan=0, pan=1, bypass, cf, bw)
   
   highcf = 5000
   cf = maketable("line", "nonorm", 2000, 0,highcf, dur/3,lowcf, dur,highcf)
   bw = maketable("line", "nonorm", 2000, 0,widebw, dur,narrowbw)
   
   start = 0.1
   FILTSWEEP(start, inskip, dur, amp * env, ringdur, steepness, balance,
      inchan=0, pan=0, bypass, cf, bw)
 
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [BUTTER](BUTTER.html),
[ELL](ELL.html), [EQ](EQ.html), [FIR](FIR.html),
[FILTERBANK](FILTERBANK.html), [FOLLOWBUTTER](FOLLOWBUTTER.html),
[IIR](IIR.html), [JFIR](JFIR.html), [MOOGVCF](MOOGVCF.html),
[MULTEQ](MULTEQ.html) [SPECTEQ](SPECTEQ.html), [SPECTEQ2](SPECTEQ2.html)
