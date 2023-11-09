---
title: DISTORT()
layout: ref
---

## DISTORT

Non-linear distortion of an input signal.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**DISTORT**(outsk, insk, dur, AMP, disttype, PREAMP, LOWPASSFREQ\[,
inputchan, PAN, BYPASS\])

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
p4 | type of distortion | 1: soft clip, 2: tube | no | no | 
p5 | gain | before distortion | yes | no | (relative multiplier | 
p6 | cutoff freq for low-pass filter | Hz | yes | no | (0 to disable filter | 
p7 | input channel |  -  | no | yes | default: 0 | 
p8 | percent to left channel |  -  | yes | yes | default: .5 | 
p9 | bypass | 0: bypass off, 1: bypass on | yes | yes | default: 0 | 

   Author: John Gibson (johgibso at indiana dot edu), 8/12/03, rev for v4, 7/10/04.

  

-----

  
**DISTORT** applies waveshaping (clipping) distortion and an optional
lowpass filter to an input sound

### Usage Notes

**DISTORT** will apply a non-linear distortion algoritm ("clipping") to
an input sound and then (optionally) lowpass filter the result. The
distortion algorithm used is taken from the [STRUM](STRUM.html)
instrument, code originally written by Charles Sullivan. **DISTORT** is
similar in action to [SHAPE](SHAPE.html).

The "tube" setting for p4 ("disttype") doesn't work correctly yet.

The optional low-pass filter (p6 \> 0) is a simple butterworth filter
design. The cutoff frequency may be modulated dynamically. The filter
comes after the distortion in the signal chain.

**DISTORT** can produce either mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("DISTORT")

   rtinput("AUDIO")
   
   // distort output
   type = 1   // 1: soft clip, 2: tube
   amp = 1.0
   ampenv = maketable("line", 1000, 0,0, 2,1, 4,1, 5,0)
   gain = 10.0
   cf = 2000 // lowpass filter
   DISTORT(0, 0, 5.0, amp*ampenv, type, gain, cf, 0, 1)
   DISTORT(0.2, 0, 5.0, amp*ampenv, type, gain, cf, 0, 0)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("WAVETABLE")
   load("DISTORT")
   
   bus_config("WAVETABLE", "aux 0 out")
   bus_config("DISTORT", "aux 0 in", "out 0-1")
   
   dur = 6.0
   amp = 10000
   pitch = 7.00
   wavetable = maketable("wave", 1000, 
      1, 1/2, 1/3, 1/4, 1/5, 1/6, 1/7, 1/8, 1/9, 1/10, 1/11, 1/12,
      1/13, 1/14, 1/15, 1/16, 1/18, 1/19, 1/20, 1/21, 1/22, 1/23, 1/24)  // saw
   reset(10000) 
   WAVETABLE(0, dur, amp, pitch, 0, wavetable)
   WAVETABLE(0, dur, amp, pitch+.0002, 0, wavetable)
   
   // distort wavetable output
   bypass = 0
   type = 1   // 1: soft clip, 2: tube
   amp = 1.2
   ampenv = maketable("line", 1000, 0,0, 1,1, 7,1, 10,0)
   gain = 30.0
   cf = 0
   DISTORT(0, 0, dur, amp*ampenv, type, gain, cf, 0, 1, bypass)
   DISTORT(0.2, 0, dur, amp*ampenv, type, gain, cf, 0, 0, bypass)
```

  

-----

### See Also

[BUTTER](BUTTER.html), [COMPLIMIT](COMPLIMIT.html),
[DECIMATE](DECIMATE.html), [SHAPE](SHAPE.html), [STRUMFB](STRUMFB.html),
[WAVESHAPE](WAVESHAPE.html)
