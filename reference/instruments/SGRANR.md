---
title: SGRANR()
layout: ref
---

## SGRANR

Stochastic granular synthesis.

*in RTcmix/insts/std/MARAGRAN*  
  

-----

##### quick syntax:

**SGRANR**(outsk, dur, amp, rate, p4-7: ratevar, p8-11: duration,
p12-15: location, p16-19: transposition\[, grainlayers (not used),
seed\])


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amplitude multiplier | relative multiplier of input signal | no | no | 
p3 | grain rate | seconds | no | no | 
p4-7 | rate variance | between 0.0 and 1.0 | no | no |  the % variation in rate - 100% being the rate |
p8-11 | duration values | seconds | no | no | 
p12-15 | location values | 0-1 stereo; 0.5 is middle | no | no | 
p16-19 | pitch values | Hz | no | no | 
p20 | grainlayers |  -  | no | yes | default: 0 | 
p21 | random number seed |  -  | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Assumes function table 1 is the amplitude envelope
   function table 2 is the synthesis waveform,
   function table 3 is grain amplitude envelope

   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Mara Helmuth (mara dot helmuth at uc dot edu)

  

-----

  
**SGRANR** does stochastic granular synthesis. It's a fairly powerful
instrument, with lots of snazzy gen-envelope controls to produce
evolving granular textures. To best understand the concepts behind the
design of this instrument, see Mara Helmth's
[papers](https://ccm.uc.edu/music/cmt/events/computermusic/software) on
the use of these techniques.

It was originally adapted from the older *sgran* Cmix instrument, also
written by Mara.

### Usage Notes

At present the setting of the buffer size in
[rtsetparams](../scorefile/rtsetparams.html) appears to have a
significant effect on the output of this instrument.

**SGRANR** produces stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("SGRANR")

   makegen(1, 7, 1000, 1, 950, 1, 50, 0)
   makegen(2, 10, 1000, 1)
   makegen(3, 7, 1000, 0, 500, 1, 500, 0)

   start = 0.0
   SGRANR(start, 9.7, 20000, 
   /* grain rate, ratevar values (must be positive, 
      % until next grain possible displacement): */
   .1, 0.1, 0.1, 0.1, 1.0,
   /* duration values: */
   .1,.1,.1,2, 
   /* location values: */
   0.0,.5,1.0,1.0, 
   /* pitch values: */
   800,1000,9700,2,
   /* granlyrs, seed */
   1,1)
```

  

-----

### See Also

[GRANULATE](GRANULATE.html), [GRANSYNTH](GRANSYNTH.html),
[JCHOR](JCHOR.html), [JGRAN](JGRAN.html), [STGRANR](STGRANR.html)
