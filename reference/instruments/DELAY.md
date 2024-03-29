---
title: DELAY()
layout: ref
---

## DELAY

Delay an input signal with feedback.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**DELAY**(outsk, insk, indur, AMP, DELAYTIME, FEEDBACK, ringdowndur\[,
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
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | delay time | seconds | yes | no | 
p5 | delay feedback | regeneration multiplier, 0-1 | yes | no | 
p6 | ring-down duration | seconds | no | no | 
p7 | input channel |  -  | no | yes | default: 0 | 
p8 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 


Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

-----

  
**DELAY** is a simple, regenerating delay instrument. It has parameters
for regeneration (amount of feedback) and for ring-down time, so that
the delay decays naturally. The original and delayed signals are mixed
in mono, but will be placed in a stereo field at a point determined by
the optional pan parameter (p8).

### Usage Notes

The point of the ring-down duration parameter is to let you control how
long the delay will sound after the input has stopped. Too short a time,
and the sound may be cut off prematurely.

**DELAY** can produce mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("DELAY")

   rtinput("somesound.aif")

   DELAY(0, 0, 7, 0.7, .14, 0.7, 3.5)
   DELAY(3.5, 0, 7, 0.9, 1.4, 0.3, 5)
```

  
  
more advanced:

```cpp
   rtsetparams(44100, 2, 512)
   load("DELAY")

   rtinput("AUDIO")

   ampenv = maketable("line", 1000, 0,0, 1,1, 90,1, 100,1)
   DELAY(0, 0, 14, 0.7*ampenv, .078, 0.8, 3.5, 0, 0.1)

   ampenv = maketable("line", 1000, 0,0, 50,1, 90,1, 100,0)
   panner = makeLFO("sine", 0.5, 0, 1)
   DELAY(7, 0, 10, 1*ampenv, .415, 0.5, 3, 0, panner)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [DEL1](DEL1.html),
[PANECHO](PANECHO.html), [COMBIT](COMBIT.html), [JDELAY](JDELAY.html)
