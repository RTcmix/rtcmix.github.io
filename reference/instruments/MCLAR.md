---
title: MCLAR()
layout: ref
---

## MCLAR

Clarinet physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MCLAR**(outsk, dur, AMP, FREQ, NOISEAMP, maxpressure, REEDSTIFF\[,
PAN, BREATHENV\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | (seconds) | no | no | 
p1 | duration | (seconds) | no | no | 
p2 | amplitude | (absolute, for 16-bit soundfiles: 0-32768) | yes | no | 
p3 | frequency | (Hz) | yes | no | 
p4 | noise gain | (0.0-1.0) | yes | no | 
p5 | max pressure | (0.0-1.0) | no | no | 
p6 | reed stiffness | (0.0-1.0) | yes | no | 
p7 | pan | (0-1 stereo; 0.5 is middle) | yes | yes | default is 0.5 | 
p8 | breath pressure envelope | reference to a pfield table-handle | yes | yes | if 0, value defaults to 1.0 | 

   Author:  Brad Garton, based on code from the Synthesis ToolKit

  

-----

  
**MCLAR** is the "Clarinet" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the
Synthesis ToolKit.

### Usage Notes

**MCLAR** implements a clarinet physical model. It seems highly similar
to the earlier [CLAR](CLAR.html) physical model, but this has better
control parameters. Also check out [MBLOWHOLE](MBLOWHOLE.html) for a
clarinet-like model with a tonehole and register vent included.

It was originally adapted from Perry Cook and Gary Scavone's
[STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), for doing
amazing physical model stuff.

This is from the orginal STK source code for "Clarinet":

Be aware that some combinations of parameters will yield no sound. The
'physics' have to work correctly\!

The optional 'breath pressure table' (p8, "BREATHENV") functions
independently of an amplitude-control envelope used in p2 ("AMP"). This
allows you to design sharp attacks, etc.

**MCLAR** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MCLAR")

   breathenv = maketable("line", 1000, 0,1, 2,0)
   MCLAR(0, 3.5, 30000.0, 314.0, 0.2, 0.7, 0.5, 0.5, breathenv)
   MCLAR(4, 3.5, 30000.0, 149.0, 0.2, 0.7, 0.5, 0.5, breathenv)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MCLAR")

   amp = maketable("line", 1000, 0,0, 1,1, 2,1, 3,0)
   freq = maketable("line", "nonorm", 1000, 0, 349, 1, 207)
   MCLAR(0, 3.5, amp*20000.0, freq, 0.2, 0.7, 0.5)

   bamp = maketable("line", 1000, 0,1, 2,1, 3,0)
   noise = makeLFO("sine", 7.0, 0.0, 1.0)
   reedstiff = maketable("line", 1000, 0,0, 1,1, 2,0)
   pan = maketable("line", 1000, 0,1, 1,0, 1.5,1, 2,0)
   MCLAR(4, 3.5, 15000.0, 149.0, noise, 0.7, reedstiff, pan, bamp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CLAR](CLAR.html),
[MLOWBOTL](MBLOWBOTL.html), [MBLOWHOLE](MBLOWHOLE.html),
[MBOWED](MBOWED.html), [MBRASS](MBRASS.html),
[METAFLUTE](METAFLUTE.html), [MSAXOFONY](MSAXOFONY.html)
