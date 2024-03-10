---
title: MBLOWBOTL()
layout: ref
---

## MBLOWBOTL

Simple helmholtz resonator physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MBLOWBOTL**(outsk, dur, AMP, FREQ, NOISEAMP, maxpressure\[, PAN,
PRESSUREENV\])

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
p3 | frequency | Hz | yes | no | 
p4 | noise gain | 0.0-1.0 | yes | no | 
p5 | max pressure | 0.0 - 1.0 | no | no | 
p6 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0.5 | 
p7 | breath pressure table | reference to a pfield table-handle | yes | yes | default: 1.0 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Brad Garton, based on code from the Synthesis ToolKit

  

-----

  
**MBLOWBOTL** is the "BlowBotl" physical model in Perry Cook and Gary
Scavone's [STK](https://www.cs.princeton.edu/~prc/NewWork.php#STK) , the
Synthesis ToolKit.

### Usage Notes

**MBLOWBOTL** is a physical model instrument that recreates the haunting
sound of a simple Helmholtz resonator. Once you hear this, you will
probably never want to use any other instrument ever again. Well, maybe
not...

Here's what Perry says about "BlowBotl":

The optional 'breath pressure table' (p7, "PRESSUREENV") functions
independently of an amplitude-control envelope used in p2 ("AMP"). This
allows you to design sharp attacks, etc.

**MBLOWBOTL** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MBLOWBOTL")

   amp = 30000
   ampenv = maketable("line", 1000, 0,1, 1,0)
   MBLOWBOTL(0, 3.5, amp*ampenv, 349.0, 0.3, 0.5)
   MBLOWBOTL(4, 3.5, amp*ampenv, 278.0, 0.7, 0.8)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MBLOWBOTL")

   ampenv = maketable("line",1000, 0,0, 1,1, 2,0)
   amp = 20000.0 * 5
   noiseamp = 0.1
   st = 0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(7.03), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(7.05), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(7.07), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(7.08), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(7.10), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(8.00), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(8.02), noiseamp, 0.9)
   st = st + 2.0
   MBLOWBOTL(st, 2.0, amp*ampenv, cpspch(8.03), noiseamp, 0.9)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CLAR](CLAR.html),
[MBLOWHOLE](MBLOWHOLE.html), [MBOWED](MBOWED.html),
[MBRASS](MBRASS.html), [MCLAR](MCLAR.html), [METAFLUTE](METAFLUTE.html),
[MSAXOFONY](MSAXOFONY.html)
