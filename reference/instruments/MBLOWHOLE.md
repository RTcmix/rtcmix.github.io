---
title: MBLOWHOLE()
layout: ref
---

## MBLOWHOLE

Waveguide clarinet phyiscal model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MBLOWHOLE**(outsk, dur, AMP, FREQ, NOISEAMP, maxpressure, REEDSTIFF,
TONEHOLE, VENT\[, PAN, PRESSUREENV\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3 = frequency (Hz)
   p4 = noise gain (0.0-1.0)
   p5 = max pressure (0.0-1.0)
   p6 = reed stiffness (0.0-1.0)
   p7 = Tonehole state (1 == "open"; 0 == "closed")
   p8 = Register vent state (1 == "open"; 0 == "closed")
   p9 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0.5]
   p10 = breath pressure table [optional; default is 1.0]

   p2 (amplitude), p3 (frequency), p4 (noise amp), p6 (reed stiffness), p7 (tonehole state),
   p8 (register vent state) and p9 (pan) can receive dynamic updates from a
   table or real-time control source.

   p10 (breath pressure table), if used, should be a reference to a pfield table-handle.

   Author:  Brad Garton, based on code from the Synthesis ToolKit
```

  

-----

  
**MBLOWHOLE** is the "BlowHole" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK) , the
Synthesis ToolKit.

### Usage Notes

**MBLOWHOLE** is a simple clarinet/reed physical model instrument very
similar in sound and action to the earlier [CLAR](CLAR.html) instrument.
The addition of a 'tonehole' and 'register vent' to the model give it a
bit more flexibility. See also the [MCLAR](MCLAR.html) instrument.

Here's what Perry and Gary say about "BlowHole":

The optional 'breath pressure table' (p10, "PRESSUREENV") functions
independently of an amplitude-control envelope used in p2 ("AMP"). This
allows you to design sharp attacks, etc.

**MBLOWHOLE** can produce other mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("MBLOWHOLE")

   amp = 20000
   ampenv = maketable("line", 1000, 0,1, 2,0)

   MBLOWHOLE(0, 3.5, amp*ampenv, 414.0, 0.2, 0.7, 0.5, 1, 1)
   MBLOWHOLE(4, 3.5, amp*ampenv, 414.0, 0.2, 0.7, 0.5, 0, 1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("MBLOWHOLE")

   amp = 20000.0
   ampenv = maketable("line", 1000, 0,0,  0.1,1,  1,1, 1.1,0)
   ventstate = makeLFO("sine", 1, 0.0, 1.0)
   MBLOWHOLE(0, 3.5, amp*ampenv, 414.0, 0.2, 0.7, 0.5, 0, ventstate)

   breathamp = maketable("line", 1000, 0,0, 1,1, 2,1, 5,0)
   freq = maketable("line", "nonorm", 1000, 0,300, 1, 500)
   noiseamp = makeLFO("saw", 8.0, 1.0, 0.0)
   pan = makeLFO("sine", 1.4, 0.0, 1.0)
   MBLOWHOLE(4, 5.2, amp*ampenv, freq, noiseamp, 0.7, 0.2, 0, 1, pan, breathamp)

   reedstiff = maketable("line", "nonorm", 1000, 0,0.6, 1,0.81, 2,0)
   tonehole = makeLFO("sine", 1, 0.0, 1.0)
   ventstate = makeLFO("sine", 3.5, 0.0, 1.0)
   MBLOWHOLE(10, 3.5, amp*ampenv, 214.0, 0.2, 0.7, reedstiff, tonehole, ventstate)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CLAR](CLAR.html),
[MBLOWBOTL](MBLOWBOTL.html), [MBOWED](MBOWED.html),
[MBRASS](MBRASS.html), [MCLAR](MCLAR.html), [METAFLUTE](METAFLUTE.html),
[MSAXOFONY](MSAXOFONY.html)