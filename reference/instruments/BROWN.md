---
title: BROWN()
layout: ref
---

## BROWN

Brown noise instrument.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**BROWN**(outsk, dur, AMP\[, PAN\])

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
p3 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: .5 | 

   Author: Neil Thornock (neilthornock at gmail), 11/12/16

  

-----

  
**BROWN** is based on an [algorithm by Andrew
Simper](http://vellocet.com/dsp/noise/VRand.html), who credits these
people, mainly from the music-dsp mailing list: Allan Herriman, James
McCartney, Phil Burk and Paul Kellet, and the [web page by Robin
Whittle](http://www.firstpr.com.au/dsp/pink-noise).

### Usage Notes

Output may be mono or stereo.

### Sample Score

```cpp
   rtsetparams(44100, 2)
   load("BROWN")

   BROWN(0, dur=100, amp=4000, pan=0.5)
```

  

-----

### See Also

[CRACKLE](CRACKLE.html), [DUST](DUST.html), [HENON](HENON.html),
[IIR](IIR.html), [LATOOCARFIAN](LATOOCARFIAN.html), [NOISE](NOISE.html),
[PINK](PINK.html)
