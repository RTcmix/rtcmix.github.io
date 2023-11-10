---
title: MSAXOFONY()
layout: ref
---

## MSAXOFONY

Saxophone physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MSAXOFONY**(outsk, dur, AMP, FREQ, NOISEAMP, maxpressure, REEDSTIFF,
APERTURE, BLOWPOS\[, PAN, BREATHENV\])

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
p2 | gain | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | frequency | Hz | yes | no | 
p4 | noise gain | 0.0-1.0 | yes | no | 
p5 | max pressure | 0.0-1.0 | no | no | 
p6 | reed stiffness | 0.0-1.0 | yes | no | 
p7 | reed aperture | 0.0-1.0 | yes | no | 
p8 | blow position | 0.0-1.0 | yes | no | 
p9 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0.5 | 
p10 | breath pressure | reference to a pfield table-handle | yes | yes | if 0, value defaults to 1.0 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Brad Garton, based on code from the Synthesis ToolKit

  

-----

  
**MSAXOFONY** is the "Saxofony" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the
Synthesis ToolKit.

### Usage Notes

**MSAXOFONY** *-- it's a late, smoky night in the Naked City. Echoing
through the alleyway, diffracted by several dozen fire escapes, comes
the throaty, creaky/squeaky sound of a lazy blues... from a laptop\!*

Well, not really -- but someday. **MSAXOFONY** implements a physical
model of a sax. Yeah, daddy-oh.

Here's what Gary says about "Saxofony":

The model is similar in many ways to the [MCLAR](MCLAR.html) and
[MBLOWHOLE](MBLOWHOLE.html) models.

Be aware that some combinations of parameters will yield no sound. The
'physics' have to work correctly\!

The optional 'breath pressure table' (p8, "BREATHENV") functions
independently of an amplitude-control envelope used in p2 ("AMP"). This
allows you to design sharp attacks, etc.

**MSAXOFONY** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MSAXOFONY")

   breathenv = maketable("line", 1000, 0,1, 2,0)
   MSAXOFONY(0, 3.5, 20000.0, 243.0, 0.2, 0.7, 0.5, 0.3, 0.6, 0.5, breathenv)
   MSAXOFONY(4, 3.5, 20000.0, 149.0, 0.2, 0.7, 0.5, 0.3, 0.6, 0.5, breathenv)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MSAXOFONY")

   noiseamp = maketable("line", 1000, 0,1, 1,0, 2,1)
   aperture = makeLFO("sine", 2.0, 0.1, 0.7)
   blowpos = maketable("line", 1000, 0,0.5, 1,0.2, 3,0.8)
   MSAXOFONY(0, 3.5, 15000.0, 243.0, noiseamp, 0.7, 0.5, aperture, blowpos)

   amp = maketable("line", 1000, 0,0,1,1, 2,0)
   freq = makeLFO("sine", 4.0, 145, 150)
   reed = makeLFO("sine", 1.5, 0.1, 0.9)
   pan = maketable("line", 1000, 0,0.5, 1,1, 2,0, 2.5,1)
   MSAXOFONY(4, 3.5, amp*10000.0, freq, 0.2, 0.7, reed, 0.3, 0.6, pan)

   bamp = maketable("line", 1000, 0,0, 2,1, 10,1, 11,0)
   MSAXOFONY(8, 3.5, 20000.0, 178.0, 0.2, 0.7, 0.5, 0.3, 0.6, 0.5, bamp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CLAR](CLAR.html),
[MLOWBOTL](MBLOWBOTL.html), [MBLOWHOLE](MBLOWHOLE.html),
[MBOWED](MBOWED.html), [MBRASS](MBRASS.html), [MCLAR](MCLAR.html),
[METAFLUTE](METAFLUTE.html)
