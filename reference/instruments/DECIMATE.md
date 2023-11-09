---
title: DECIMATE()
layout: ref
---

## DECIMATE

Bit-reduce an input signal.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**DECIMATE**(outsk, insk, dur, PREAMP, POSTAMP, NBITS\[, LOWPASSFREQ,
inputchan, PAN\])

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
p3 | pre-amp multiplier (before decimation) | relative multiplier | yes | no |
p4 | post-amp multiplier (after decimation) | relative multiplier | yes | no |
p5 | number of bits to use | 1 to 16 | yes | no | 
p6 | low-pass filter cutoff frequency | Hz | yes | yes | default: 0 (will bypass) | 
p7 | input channel |  -  | no | yes | default: 0 | 
p8 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 

   p3 (pre-amp), p4 (post-amp), p5 (bits), p6 (cutoff) and p8 (pan) can
   receive dynamic updates from a table or real-time control source.

   JGG , 3 Jan 2002, rev for v4, 7/11/04

  

-----

  
**DECIMATE** reduces the number of bits in the input audio signal

### Usage Notes

**DECIMATE** reduces the number of bits used to represent the amplitude
of individual samples. The sound quality will be altered as a result

The optional low-pass filter (p5 \> 0) is a simple butterworth filter
design. The cutoff frequency may be modulated dynamically.

The "PREAMP" (p3) and "POSTAMP" (p4) parameters allow you to adjust the
alteration in amplitude resulting from the bit-rediction.

The output of **DECIMATE** can be either mono or stereo.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("DECIMATE")

   rtinput("mysound.aif")

   bits = 2
   cutoff = 4000
   dur = DUR()
   amp = 1
   ampenv = maketable("line", 1000, 0,0, 1,1, 5,1, 10,0)

   DECIMATE(0, 0, dur, 1, amp*ampenv, bits, cutoff)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("DECIMATE")
   
   rtinput("mysound.aif")
   
   inchan = 0
   dur = DUR()
   
   bits = 2
   preamp = 2
   postamp = maketable("line", 1000, 0,0, 5,1, 9,1, 10,0)
   cutoff = maketable("line", "nonorm", 1000, 0,1, 1,10000, 2,800)
   pan = maketable("line", 100, 0,0, 1,1)
   
   DECIMATE(0, 0, dur, preamp, 0.9*postamp, bits, cutoff, inchan, pan)
```

  

-----

### See Also

[BUTTER](BUTTER.html), [COMPLIMIT](COMPLIMIT.html),
[COMPLIMIT](COMPLIMIT.html), [DISTORT](DISTORT.html),
[SHAPE](SHAPE.html)
