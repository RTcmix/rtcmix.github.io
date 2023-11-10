---
title: MOCKBEND()
layout: ref
---

## MOCKBEND

Cubic spline dynamic pitch-shifter.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**MOCKBEND**(outsk, insk, dur, amp, pitchenvgenno\[, inputchan, pan\])


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | output duration (or endtime if negative) | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | no | no | 
p4 | function table number for pitch envelope |  -  | no | no | 
p5 | input channel |  -  | no | yes | default: 0 | 
p6 | percent to left |  -  | no | yes | default: .5 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Assumes function table 1 is the amplitude envelope

   Author:  Ivica Ico Bukvic (based on Doug Scott's TRANSBEND instrument)

  

-----

  
**MOCKBEND** performs a time-varying pitch transposition on a mono input
signal (channel-selectable) using cubic spline interpolation

### Usage Notes

**MOCKBEND** is a version of [TRANSBEND](TRANSBEND.html) designed to
work with real-time input sources, such as a microphone or aux bus. It
processes only one channel at a time.

**MOCKBEND** uses the older [makegen](../scorefile/makegen.html) control
envelope system to specify the pitch-transposition envelope. It uses the
function table specified by p4 for this data. The interval values in
this table are expressed in linear octaves ([makegen(2,
...)](../scorefile/gen2.html) is probably best for this).

**MOCKBEND** can produce either mono or stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2)
   load("MOCKBEND")

   rtinput("mysoundfile.aif")
   
   dur = DUR(0)
   amp = 1.5
   pan = 0.5
   
   /* amplitude curve */
   setline(0,0, 1,1, 90,1, 100,0)
   
   /* transpose from 4 semitones up to 8 down - stored in gen slot 2 */
   makegen(-2, 18, 512, 1,.4, 512,-.8)
   
   MOCKBEND(0, 0, dur, amp, 2, 0, pan)
```

  

-----

### See Also

[PVOC](PVOC.html), [SCRUB](SCRUB.html), [TRANS](TRANS.html),
[TRANS3](TRANS3.html), [TRANSBEND](TRANSBEND.html)
