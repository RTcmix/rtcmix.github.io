---
title: COMBIT()
layout: ref
---

## COMBIT

Comb filter an input signal.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**COMBIT**(outsk, insk, dur, AMP, FREQ (Hz), REVERBTIME\[, inputchan,
PAN, ringdowndur\])

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
p4 | frequency | Hz | yes | no | 
p5 | reverb time | seconds | yes | no | 
p6 | input channel |  -  | no | yes | default: 0 | 
p7 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 
p8 | ring-down duration |  -  | no | yes | default: initial p5 value | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

-----

  
**COMBIT** applies a comb filter -- a short feedback delay line -- to an
input signal. This delay line of delay time *T*, when applied to an
incoming signal, causes the sound to ring at frequency *1/T* for an
amount of time which decays exponentially proportional to the percentage
of feedback (adapted from Roads, 1997). **COMBIT** thus takes an input
soundfile or real-time audio input and makes it ring at the frequency
specified in p4 of the instrument, with a decay time in seconds of p5.
The efficacy of the comb filter is dependent on the amount of that
frequency already present in the incoming signal (thus, a very low
filter frequency applied to a high-pitched soundfile will not ring as
well as it would applied to a low-sounding signal).

### Usage Notes

The point of the ring-down duration parameter (p8, optional) is to let
you control how long the combs will ring after the input has stopped. If
the reverb time is constant, **COMBIT** will figure out the correct
ring-down duration for you based on the reverb time (p5). If the reverb
time is dynamic, you must specify a ring-down duration if you want to
ensure that your sound will not be cut off prematurely.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("COMBIT")

   rtinput("somesound.aif")

   // two resonating filters a fifth apart in separate channels
   // the sound of the second will be slightly delayed (0.2 seconds)
   COMBIT(0, 0, 3.5, 0.3, cpspch(7.09), .5, 0, 0)
   COMBIT(0.2, 0, 3.5, 0.3, cpspch(7.07), .5, 0, 1)
```

  
  
more advanced:

```cpp
   rtsetparams(44100, 2)
   load("COMBIT")
   control_rate(1000)

   rtinput("AUDIO")

   dur = 0.1
   ampenv = maketable("line", 1000, 0,0, 0.1,1, 1,0) 
   for (outsk = 0; outsk < 14.0; outsk = outsk + 0.1) {
      insk = random() * 7.0
      pitch = random() * 500 + 100
      COMBIT(outsk, insk, dur, 0.1*ampenv, pitch, .5, 0, random());
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [MULTICOMB](MULTICOMB.html),
[FLANGE](FLANGE.html), [DELAY](DELAY.html), [JDELAY](JDELAY.html),
[REVERBIT](REVERBIT.html), [REV](REV.html), [IIR](IIR.html)
