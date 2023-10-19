---
title: TRANS3()
layout: ref
---

## TRANS3

Pitch-transposiion using 3rd-order interpolation.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**TRANS3**(outsk, insk, dur, AMP, TRANSP\[, inputchan, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = output duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = interval of transposition (oct.pc)
   p5 = input channel [optional; default is 0]
   p6 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (amplitude), p4 (transposition) and p6 (pan) can receive dynamic updates
   from a table or real-time control source.

   Author: Doug Scott; rev. for v4 by JG, 2/11/06
```

  

-----

  
**TRANS3** transposes the input for the specified output duration (p2),
starting at the input start time (p1). It has the same syntax as
[TRANS](TRANS.html) and is used in the same fashion. The difference is
that **TRANS3** uses 3rd-order interpolation to achieve the pitch-shift.
This is s a bit more expensive computationally, but it does produce
smoother results.

### Usage Notes

The use of **TRANS3** is identical to the [TRANS](TRANS.html)
instrument. See the [TRANS Usage Notes](TRANS.html#usage_notes) for more
information.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("TRANS") // note that TRANS3 is loaded in the TRANS library

   rtinput("mysound.aif")

   trans = -0.02
   // do both channels of a stereo input file
   TRANS3(outskip=1, inskip=2, dur=4, amp=1, trans, inchan=0, pan=0)
   TRANS3(outskip=1, inskip=2, dur=4, amp=1, trans, inchan=1, pan=1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("TRANS")

   rtinput("mysound.aif")

   start = 0
   inskip = 0
   dur = 7.8
   amp = 1.0
   ampenv = maketable("line", 1000, 0,0, 1,1, 7,1, 7.8,0)

   low = octpch(-0.05)
   high = octpch(0.03)
   transp = maketable("line", "nonorm", 1000, 0,0, 1,low, 3,high)
   transp = makeconverter(transp, "pchoct")

   /*
   This transposition starts at 0, moves down by a perfect fourth (-0.05),
   then up to a minor third (0.03) above the starting transposition.  The
   table is expressed in linear octaves, then converted to octave.pc by the
   call to makeconverter.
   */
   TRANS3(start, inskip, dur, amp*ampenv, transp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconverter](../scorefile/makeconverter.html),
[MOCKBEND](MOCKBEND.html), [SCRUB](SCRUB.html), [TRANS](TRANS.html),
[TRANSBEND](TRANSBEND.html), [PVOC](PVOC.html)
