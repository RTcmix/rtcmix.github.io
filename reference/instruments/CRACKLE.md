---
title: CRACKLE()
layout: ref
---

## CRACKLE

Chaotic noise generator.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**CRACKLE**(outsk, dur, AMP\[, CHAOS, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0 = output start time
   p1 = duration
   p2 = amplitude multiplier
   p3 = chaos parameter (0-1) [optional, default is 1]
   p4 = pan (in percent-to-left format) [optional, default is .5]

   p2 (amplitude), p3 (chaos), and p4 (pan) can receive updates from a table or
   real-time control source.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16
```

  

-----

  
Chaos (p3) values near 0 are less crackly (closer to white noise). As
chaos approaches 1, it crackles more.

### Usage Notes

Output may be mono or stereo.

### Sample Score

```cpp
   rtsetparams(44100, 2)
   load("CRACKLE")

   CRACKLE(0, dur=100, amp=4000, chaos=0.9, pan=0.5)
```

  

-----

### See Also

[BROWN](BROWN.html), [DUST](DUST.html), [HENON](HENON.html),
[IIR](IIR.html), [LATOOCARFIAN](LATOOCARFIAN.html), [NOISE](NOISE.html),
[PINK](PINK.html)
