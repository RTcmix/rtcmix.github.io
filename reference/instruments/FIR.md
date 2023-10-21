---
title: FIR()
layout: ref
---

## FIR

Simple finite impulse response filter.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**FIR**(outsk, insk, dur, AMP, ncoefficients, coefficients...)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = total number of filter-equation coefficients
   p5...  the coefficients (up to 99 fir coefficients)

  p3 (amplitude) can receive updates.
  NOTE: mono input / mono output only
```

  

-----

  
**FIR** creates a *finite impulse response* filter using the filter
coefficients passed to the instrument. The filter is non-recursive (i.e.
only past inputs are used in the equation), in the form:  

### Usage Notes

**FIR** can be used to design fairly complex filters, although the
phase-shifting due to a long stream of past inputs is an inherent design
problem with basic FIR filters. The trick is to find the right set of
coefficients. There are a number of good web sites that will generate
FIR coefficients given a frequency-graph type of specification.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("FIR")

   rtinput("mysound.aif")
   FIR(0, 0, 7.0, 0.7, 7, 0.9, 0.1, 0.69, -0.49, 0.314, 0.2, 0.09)
```

  

-----

### See Also

[BUTTER](BUTTER.html) [ELL](ELL.html) [EQ](EQ.html)
[FILTERBANK](FILTERBANK.html), [FILTSWEEP](FILTSWEEP.html),
[IIR](IIR.html), [JFIR](JFIR.html), [MOOGVCF](MOOGVCF.html),
[MULTEQ](MULTEQ.html)
