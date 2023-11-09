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


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time |  -  | no | no | 
p1 | duration |  -  | no | no | 
p2 | amplitude multiplier |  -  | yes | no | 
p3 | a |  -  | yes | yes | default is 1.4 | 
p4 | b |  -  | yes | yes | default is 0.3 | 
p5 | x |  -  | yes | yes | default is 1 | 
p6 | y |  -  | yes | yes | default is 1 | 
p7 | update rate for p3-p6 | | times per second  | yes | yes | default is 1000 | 
p8 | pan | percent-to-left | yes | yes | default is .5 | 

   p3-p6: Try values within a few tenths of the defaults given here.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16.

  

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
