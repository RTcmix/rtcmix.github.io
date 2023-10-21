---
title: HENON()
layout: ref
---

## HENON

Henon map noise generator.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**HENON**(outsk, dur, AMP\[, A, B, X, Y, UPDATE, PAN)

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
   p3 = a [optional, default is 1.4]
   p4 = b [optional, default is 0.3]
   p5 = x [optional, default is 1]
   p6 = y [optional, default is 1]
   p7 = update rate for p3-p6 [optional, default is 1000]
   p8 = pan (in percent-to-left format) [optional, default is .5]

   p2 (amp), p3-p6 (function parameters), p7 (update rate), and p8 (pan)
   can receive updates from a table or real-time control source.

   p3-p6: Try values within a few tenths of the defaults given here.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16.
```

  

-----

  
Function from the Henon map by Michel Henon.

### Usage Notes

HENON is a chaotic noise generator. Pfields p3-p6 default to classical
Henon map values. Varying these parameters will lead to a variety of
different results. Try values within a few tenths of the defaults given.

Pfield p7 (update rate) is how many times per second p3-p6 are updated
and affects the frequency. Values greater than 0 and less than sample
rate divided by 5 are valid.

### Sample Score

```cpp
   rtsetparams(44100, 2)
   load("HENON")

   HENON(0, dur=100, amp=4000, a=1.4, b=0.3, x=1, y=1, update=1000, pan=0.5)
```

  

-----

### See Also

[BROWN](BROWN.html), [CRACKLE](CRACKLE.html), [DUST](DUST.html),
[IIR](IIR.html), [LATOOCARFIAN](LATOOCARFIAN.html), [NOISE](NOISE.html),
[PINK](PINK.html)
