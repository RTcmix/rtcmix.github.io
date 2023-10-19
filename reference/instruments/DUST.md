---
title: DUST()
layout: ref
---

## DUST

Random impulses.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**DUST**(outsk, dur, AMP\[, DENSITY, min range, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time
   p1 = duration
   p2 = amplitude multiplier
   p3 = density (average impulses per second) [default: 5]
   p4 = impulse range minimum (-1 or 0) [default: -1]
   p5 = seed [default: system clock]
   p6 = pan (in percent-to-left format) [default: 0.5]

   p2 (amplitude), p3 (pan), and p4 (density) can receive updates from a table
   or real-time control source.
   
   Author: Neil Thornock (neilthornock at gmail), 11/12/16
```

  

-----

  
**DUST** generates randomly spaced impulses with a range of either 0 to
1 or -1 to 1. At very high densities, it generates white noise.

Inspired by the Dust and Dust2 ugens from SuperCollider.

### Usage Notes

Output may be mono or stereo.

### Sample Score

``` 
   rtsetparams(44100, 2)
   load("DUST")

   density = maketable("line", "nonorm", 1000, 0,0, 1,10)
   DUST(0, dur=100, amp=4000, density, minrange=-1, pan=0.5)
```

  

-----

### See Also

[BROWN](BROWN.html), [CRACKLE](CRACKLE.html), [HENON](HENON.html),
[IIR](IIR.html), [LATOOCARFIAN](LATOOCARFIAN.html), [NOISE](NOISE.html),
[PINK](PINK.html)
