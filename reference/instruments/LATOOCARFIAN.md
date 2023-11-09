---
title: LATOOCARFIAN()
layout: ref
---

## LATOOCARFIAN

Chaotic noise generator.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**LATOOCARFIAN**(outsk, dur, AMP\[, A, B, C, D, X, Y, PAN\])

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
p3 | parameter a |  -  | yes | yes | default: 2.871 | 
p4 | parameter b |  -  | yes | yes | default: 2.75 | 
p5 | parameter c |  -  | yes | yes | default: 1 | 
p6 | parameter d |  -  | yes | yes | default: 0.75 | 
p7 | seed x |  -  | yes | yes | default: 0.5 | 
p8 | seed y |  -  | yes | yes | default: 0.5 | 
p9 | pan | in percent-to-left format | yes | yes | default: .5 | 

   Any values for p3-p8 are legal. Pickover recommends values for a and b
   between -3 and 3,  and p4 and p5 between 0.5 and 1.5. Depending on the
   values provided, results may be chaotic noise, pitch, or silence.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16

  

-----

  
**LATOOCARFIAN** is based on a function given in Clifford Pickover's
book Chaos in Wonderland.

### Usage Notes

The Latoocarfian chaotic function was described by Clifford Pickover in
his book Chaos in Wonderland. Any values for p3-p8 are legal, but not
all values will produce sonic results. Try values of -3 to 3 for p3 and
p4, and values of 0.5 to 1.5 for p4 and p5.

### Sample Score

```cpp
   rtsetparams(44100, 2)
   load("LATOOCARFIAN")

   c = maketable("line", 1000, 0,0.5, 1,1)
   LATOOCARFIAN(0, dur=100, amp=4000, a=2.871, b=2.75, c, d=0.75, x=0.5, y=0.5,
      pan=0.5)
```

  

-----

### See Also

[BROWN](BROWN.html), [CRACKLE](CRACKLE.html), [DUST](DUST.html),
[HENON](HENON.html), [IIR](IIR.html), [NOISE](NOISE.html),
[PINK](PINK.html)
