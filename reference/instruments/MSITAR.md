---
title: MSITAR()
layout: ref
---

## MSITAR

Sitar physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MSITAR**(outsk, dur, AMP, FREQ, plamp\[, PAN, AMPENV\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3 = frequency (Hz)
   p4 = pluck amp (0.0-1.0)
   p5 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0.5]
   p6 = amplitude envelope table [optional; default is 1.0]

   p2 (amplitude), p3 (frequency) and p5 (pan) can receive dynamic updates from
   a table or real-time control source.

   p6 (amplitude envelope table), if used, should be a reference to a pfield table-handle.

   Author:  Brad Garton, based on code from the Synthesis ToolKit
```

  

-----

  
**MSITAR** the "Sitar" physical model in Perry Cook and Gary Scavone's
[STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the Synthesis
ToolKit.

### Usage Notes

**MSITAR** models an Indian sitar. It uses a modified Karplus-Strong
("plucked string" -- see the [STRUM2](STRUM2.html) instrument)
algorithm. Here's what Perry says about "Sitar":

The amplitude envelope table (p6, "AMPENV") is an optional parameter. It
is also redundant -- the same functionality can be achieved by using a
pfield-table reference in p2 ("AMP").

**MSITAR** can produce other mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1)
   load("MSITAR")

   amp = 17000
   MSITAR(0, 3.5, amp, cpspch(8.00), 0.9)
   MSITAR(4, 3.5, amp, cpspch(8.07), 0.9)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("MSITAR")

   MSITAR(0, 3.5, 30000, cpspch(8.00), 0.9)

   amp = maketable("line", 1000, 0,0, 1,1, 2,0)
   freq = makerandom("linear", 9.0, cpspch(8.065), cpspch(8.075))
   MSITAR(4, 3.5, amp*20000, freq, 0.9, 0.0)
   freq = makerandom("linear", 7.0, cpspch(8.065), cpspch(8.075))
   MSITAR(4, 3.5, amp*20000, freq, 0.9, 1.0)

   stramp = maketable("line", 1000, 0,1, 2,0)
   pan = makeLFO("sine", 14, 0, 1)
   MSITAR(8, 3.5, 20000, cpspch(7.07), 0.9, pan, stramp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CLAR](CLAR.html),
[MBLOWBOTL](MBLOWBOTL.html), [MBLOWHOLE](MBLOWHOLE.html),
[MBOWED](MBOWED.html), [MBRASS](MBRASS.html), [MCLAR](MCLAR.html),
[METAFLUTE](METAFLUTE.html), [MSAXOFONY](MSAXOFONY.html)
