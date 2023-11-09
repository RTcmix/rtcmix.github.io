---
title: HOLO()
layout: ref
---

## HOLO

Stereo FIR filter to perform crosstalk cancellation.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**HOLO**(outsk, insk, dur, AMP, XTALKMULT)

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
p2 | duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | crosstalk amplitude multipler |  -  | yes | no | 

   Author: Doug Scott

  

-----

  
**HOLO** is a simulation of the fun Carver Sonic Hologram Generator
(yeah, we grew up in the late 70's...). It performs crosstalk cancellation 
across the two outputs of a stereo signal (as produced by a pair of stereo
speakers).

### Usage Notes

Try it out to see how it affects the stereo 'image'. It can be used to
create the sense of a wide stereo field. A good use of this is as a follow-up
instrument (in the signal path) after any reverb, room simulator, or sound
placement instrument.  Note that it does *not* work well with headphones.

**HOLO** only operates on stereo input files and only writes stereo
output files.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("HOLO")

   rtinput("mysoundfile.aiff")

   HOLO(0, 0, 7.8, 0.7, 0.5)
```

  

-----

### See Also

[MIX](MIX.html), [STEREO](STEREO.html), [DMOVE](DMOVE.html)

