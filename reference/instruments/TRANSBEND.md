---
title: TRANSBEND()
layout: ref
---

## TRANSBEND

Time-varying pitch transposition.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**TRANSBEND**(outsk, insk, dur, amp, pitchenvgenno\[, inputchan, pan\])

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = output duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = function table number for pitch transposition control envelope
   p5 = input channel [optional; default is 0]
   p6 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   assumes function table 1 is the amplitude envelope

   Author: Doug Scott 9/3/2000
```

  

-----

  
**TRANSBEND** performs a time-varying transposition on an input signal
using cubic spline interpolation. This is essentially the same thing as
[TRANS](TRANS.html), except that you can change the transposition factor
dynamically using the older [makegen](../scorefile/makegen.html) control
envelope sysystem.

### Usage Notes

Because of the new [pfield-enabled](pfield-enabled.html) control
envelope system, **TRANSBEND** has now been largely superceded by the
[TRANS](TRANS.html) and [TRANS3](TRANS3.html) instruments. The paramters
of **TRANSBEND** are the same as with these instruments. See the [TRANS
Usage Notes](TRANS.html#usage_notes) for more information.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("TRANSBEND")
   
   rtinput("mysound.snd")
   
   dur = DUR(0)
   amp = 1
   pan = 0.5
   
   /* amplitude curve */
   setline(0,0, 1, 1, 90, 1, 100, 0)
   
   /* transpose from 4 semitones up to 8 down - stored in gen slot 2 */
   makegen(-2, 18, 512, 1,.04, 512,-.08)
   
   TRANSBEND(0, 0, dur, amp, 2, 0, pan)
```

  

-----

### See Also

[MOCKBEND](MOCKBEND.html), [SCRUB](SCRUB.html), [TRANS](TRANS.html),
[TRANS3](TRANS3.html), [PVOC](PVOC.html)
