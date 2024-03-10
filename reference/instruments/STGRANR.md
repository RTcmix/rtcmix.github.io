---
title: STGRANR()
layout: ref
---

## STGRANR

Sampling stochastic granular processor.

*in RTcmix/insts/std/MARAGRAN*  
  

-----

##### quick syntax:

**STGRANR**(outsk, insk, dur, amp, rate, p5-8: ratevar, p9-12: duration,
p13-16: location, p17-20: transposition\[, grainlayers (not used),
seed\])


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | no | no | 
p4 | grain rate | seconds | no | no | 
p5-8 | rate variance | between 0.0 and 1.0; it is the % variation in rate - 100% being the rate | no | no | 
p9-12 | duration values | seconds | no | no | 
p13-16 | location values | 0-1 stereo; 0.5 is middle | no | no | 
p17-20 | transposition values |  -  | no | no | 
p21 | grainlayers |  -  | no | yes | default: 0 | 
p22 | random number seed |  -  | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Assumes function table 1 is the amplitude envelope,
   function table 2 is the synthesis waveform,
   function table 3 is grain amplitude envelope

   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Mara Helmuth (mara dot helmuth at uc dot edu)

  

-----

  
**STGRANR** does stochastic granular signal-processing, decomposing an
input soundfile or real-time sound source. It's a fairly powerful
instrument, with lots of snazzy gen-envelope controls to produce
evolving granular textures. To best understand the concepts behind the
design of this instrument, see Mara Helmth's
[papers](https://ccm.uc.edu/music/cmt/events/computermusic/software) on
the use of these techniques.

It was originally adapted from the older *stgran* Cmix instrument, also
written by Mara.

### Usage Notes

At present the setting of the buffer size in
[rtsetparams](../scorefile/rtsetparams.html) appears to have a
significant effect on the output of this instrument.

**STGRANR** will take mono or stereo input files, it requires stereo
output.

### Sample Scores

very basic:

```cpp
   set_option("FULL_DUPLEX_ON")
   rtsetparams(44100, 2)
   load("STGRANR")

   rtinput("AUDIO","MIC",2)

   makegen(1, 7, 1000, 1, 950, 1, 50, 0)
   makegen(2, 25, 1000, 1)

   start = 0.0

   /* p0start, p1inputstt, p2dur, p3amp */
   STGRANR(start, 0, 13, 5000, 
   /* grain rate, ratevar values (must be positive,
      % until next grain possible displacement): */
   .1, 0.0, 0.1, 0.2, 1.0,
   /* duration values: */
   .1,.1,.1,2, 
   /* location values: */
   0.0,0.5,1.0,10.0, 
   /* pitch values: */
   0.0,0.00,0.07,2,
   /* granlyrs, seed */
   1,1)
```

  

-----

### See Also

[GRANSYNTH](GRANSYNTH.html), [GRANULATE](GRANULATE.html),
[JCHOR](JCHOR.html), [JGRAN](JGRAN.html), [SGRANR](SGRANR.html)
