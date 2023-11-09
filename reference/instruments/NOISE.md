---
title: NOISE()
layout: ref
---

## NOISE

Generate white noise.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**NOISE**(outsk, dur, AMP\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 

   Author: JGG , 24 Dec 2002, rev. 7/9/04

  

-----

  
**NOISE** makes white (or pseudo-white...) noise. It is very noisy. It
is good for annoying your family, unless your family is a bunch of
*weirdoes*.

### Usage Notes

The series of random numbers that makes the noise is affected by any
calls to the [srand](../scorefile/srand.html) scorefile command in the
script. If there are no such calls, the random seed is 1.

Output may be mono or stereo.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("NOISE")

   ampenv = maketable("window", 1000, "hanning")
   NOISE(0.0, 2.5, 20000*ampenv)
```

  

-----

### See Also

[BROWN](BROWN.html), [CRACKLE](CRACKLE.html), [DUST](DUST.html),
[HENON](HENON.html), [IIR](IIR.html), [LATOOCARFIAN](LATOOCARFIAN.html),
[PINK](PINK.html)
