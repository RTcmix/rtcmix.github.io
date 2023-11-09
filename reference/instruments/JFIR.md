---
title: JFIR()
layout: ref
---

## JFIR

FIR filter specified by frequency-response curve.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**JFIR**(outsk, insk, dur, AMP, filtorder, inputchan, PAN, BYPASS,
FREQTABLE)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | (seconds) | no | no | 
p1 | input start time | (seconds) | no | no | 
p2 | input duration | (seconds) | no | no | 
p3 | amplitude multiplier | (relative multiplier of input signal) | yes | no | 
p4 | filter order | (higher order allows steeper slope) | no | no | 
p5 | input channel |  -  | no | no | default: 0
p6 | pan | (0-1 stereo; 0.5 is middle) | yes | no | 
p7 | bypass filter | (0: bypass off, 1: bypass on) | yes | no | usually use 0 | 
p8 | frequency response table | reference to a pfield table-handle | yes | no | 

   p3 (amplitude), p6 (pan) and p7 (bypass) can receive dynamic updates from
   a table or real-time control source.

   p8 should be a reference to a pfield table-handle.

   Author:  John Gibson, 7/3/99; rev for v4, JGG, 7/24/04
   Filter design code adapted from Bill Schottstaedt's Snd.

  

-----

  
**JFIR** creates an FIR filter based on a frequency response curve in a
table referenced by p8 ("FREQTABLE"). The filter design code adapted
from Bill Schottstaedt's
[Snd](http://www-ccrma.stanford.edu/software/snd/) soundfile editor.

### Usage Notes

The filter order (p4, "filtorder") can be a fairly large number. See the
sample scores below.

The desired frequency response curve is described by a table
specification of pairs. Frequency is in Hz, from 0 to Nyquist; amp is
from 0 to 1. Ideally, frequencies with amplitude of 1 are passed without
attenuation; those with amplitude of 0 are attenuated totally. But this
behavior depends on the order of the filter. Try an order of 200, and
increase that as needed.

Example:

With a high order, this should attenuate everything below 200 Hz and
above 4000 Hz. NOTE: Be sure to use the "nonorm" option for
[maketable](../scorefile/maketable.html) when specifying the frequency
response curve.

**JFIR** Can only process 1 channel at a time. To process stereo, call
twice -- once with inputchan=0 (p5) and PAN=0 (p6), again with
inputchan=1 and PAN=1.

**JFIR** can produce either stereo or mono output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("JFIR")
   
   rtinput("mysound.aif")
   inchan = 0
   inskip = 0
   dur = DUR()
   
   start = 0
   amp = 2.5
   order = 300
   
   nyq = 44100 / 2
   freqresp = maketable("line", "nonorm", 5000,
      0,0, 100,0, 200,1, 700,1, 1000,0, 1500,0, 1600,.8, 2200,.8, 4000,0, nyq,0)
   
   env = maketable("line", 1000, 0,0, 1,1, 7,1, 9,0)
   pan = 0.5
   bypass = 0
   
   JFIR(start, inskip, dur, amp * env, order, inchan, pan, bypass, freqresp)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("JFIR")
   
   rtinput("mysound.aif")
   inchan = 0
   inskip = 0.7
   filedur = DUR() - 0.7
   
   reset(4000)
   env = maketable("line", 12000, 0,0, 1,1, 29,1, 30,0)
   
   amp = 4.0
   dur = 0.05
   pan = 0.5
   bypass = 0
   
   cflist = { 500, 2000, 1000, 750, 3000, 180, 1700, 5000, 1400, 450, 900, 2200 }
   numcf = len(cflist)
   
   half_bandwidth_percent = 0.50
   
   nyquist = 44100 / 2
   order = 300
   
   for (st = 0; st < filedur - dur; st += dur) {
      n = (st / filedur) * (numcf - 1)
      cf = cflist[trunc(n)]
      low = cf - (cf * half_bandwidth_percent)
      high = cf + (cf * half_bandwidth_percent)
      freqresp = maketable("line", 5000, 0,0, low,0, cf,1, high,0, nyquist,0)

      JFIR(st, inskip, dur, amp * env, order, inchan, pan, bypass, freqresp)
    inskip += dur
   }
```

  

-----

### See Also

[BUTTER](BUTTER.html) [ELL](ELL.html) [EQ](EQ.html)
[FILTERBANK](FILTERBANK.html), [FILTSWEEP](FILTSWEEP.html),
[FIR](FIR.html), [IIR](IIR.html), [MOOGVCF](MOOGVCF.html),
[MULTEQ](MULTEQ.html)
