---
title: SCRUB()
layout: ref
---

## SCRUB

Dynamically transpose an input signal using sync interpolation.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**SCRUB**(outsk, insk, outdur, AMP, SPEEDMULT, syncwidth,
oversampling\[, inputchan, pan\])

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
p2 | output duration (or endtime if negative) | seconds| no | no |
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | speed multiplier | linear multiple, -inf - inf | yes | no | 
p5 | sync width | samples | no | no | 
p6 | sync oversampling | samples | no | no | 
p7 | input channel |  -  | no | yes | default: 0 | 
p8 | pan |  percent to left | no | yes | default: .5 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Doug Scott, based on interpolation and i/o code by Tobias Kunze-Briseno.

  

-----

  
**SCRUB** is a pitch-shifting instrument using an interesting
interpolation scheme. This approach allows for the 'scrubbing' of sound
samples down to 0 (and reversed\!) to achieve an effect similar to the
sound of analog tape being moved back and forth against a playback tape
head.

### Usage Notes

The main parameter to use is the "SPEEDMULT" (p4), which can change
dynamically. A value of 1.0 will play the input sound normally, a value
of -1.0 will play it in reverse (you will need to set the input start
time (p1) accordingly). 2.0 will play the file at twice the speed, and
-0.5 will play it backwards at 1/2 speed. A valu of 0 will stop the
sound altogether, just like 'scrubbing' a tape on a tape head (ancient
people can recall this...).

The "syncwidth" (p5) and "oversampling" (p6) parameters determine how
the buffer used for scrubbing will be set up. These don't seem to have
an obvious effect on the sound. It also seems that they should be \< the
buffer size set in [rtsetparams](../scorefile/rtsetparams.html).

The buffer size set in [rtsetparams](../scorefile/rtsetparams.html)
seems to have a significant effect on the sound.

The output of **SCRUB** can be either mono or stereo

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("SCRUB")
   
   rtinput("mysound.aif")

   speed = 1
   dur = DUR(0)
   skip = 0
   // Play forward from time 0
   SCRUB(0, skip, dur, 1, speed, 16, 40)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2, 2048)
   load("SCRUB")

   rtinput("mysound.wav")

   speedchange = maketable("line", 8192, 0,1, 1,-1)
   dur = DUR(0) * 2
   skip = 0
   // Play file, moving from normal speed to reverse normal
   SCRUB(0, skip, dur, 1, speedchange, 16, 40)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2, 256)
   load("SCRUB")

   rtinput("mysound.aif")

   speed = -1
   skip = DUR(0)
   dur = skip
   // Play backwards from end to beginning in channel 0
   SCRUB(0, skip, dur, 1, speed, 16, 40, 0, 0)

   speed = -0.5
   skip = DUR(0)
   dur = DUR(0) * 2
   // Play reversed at half-speed from end in channel 1
   SCRUB(0, skip, dur, 1, speed, 16, 40, 0, 1)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [TRANS](TRANS.html),
[TRANS3](TRANS3.html), [TRANSBEND](TRANSBEND.html),
[MOCKBEND](MOCKBEND.html), [REVMIX](REVMIX.html), [MIX](MIX.html),
[STEREO](STEREO.html)
