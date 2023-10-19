---
title: DCBLOCK()
layout: ref
---

## DCBLOCK

Remove (most of) DC bias from input signal.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**DCBLOCK**(outsk, insk, dur, AMP)

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
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.

   Author:  John Gibson , 5/21/06
```

  

-----

  
**DCBLOCK** uses a simple one-pole/one-zero filter set to remove a DC (0
Hz) offset component in the output signal. The recursive filter equation
used for this object is:

where *y\[n\]* and *y\[n-1\]* are the current and previous outputs of
the equation, respectively, and *x\[n\]* and *x\[n-1\]* are the current
and previous sample inputs to the filter equation.

### Usage Notes

**DCBLOCK** processes N input channels to N output channels, e.g. mono
to mono, stereo to stereo, quad to quad, etc.

The sound itself should be relatively unchanged by **DCBLOCK**.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("DCBLOCK")

   rtinput("mysound.aif")

   DCBLOCK(0, 0, DUR(), 1.0)
```

  

-----

### See Also

[ELL](ELL.html), [EQ](EQ.html), [FIR](FIR.html), [IIR](IIR.html),
[JFIR](JFIR.html), [MULTEQ](MULTEQ.html)
