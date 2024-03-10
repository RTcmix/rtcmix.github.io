---
title: Oonepole()
layout: ref
---

### Oonepole

*INSTRUMENT design -- simple one-pole filter object*  
  
The **Oonepole** object instantiates a simple one-pole IIR filter, based
on designs found in *<u>Computer Music: Synthesis, Composition, and
Performance</u> by Charles Dodge and Thomas Jerse and the [Synthesis
ToolKit (STK)](https://www.cs.princeton.edu/~prc/NewWork.php#STK)
authored by Perry Cook and Gary Scavone. The general recursive filter
equation used for this object is:*

    y[n] = a*x[n] + b*y[n-1]

where *y\[n\]* and *y\[n-1\]* are the current and previous outputs of
the equation, respectively, and *x\[n\]* is the current input. *a* and
*b* are the coefficients to the equation that determine what kind of
filter is constructed. Simple low-pass and high-pass filters are
possible with characteristics depending upon the values of the *a* and
*b* coefficients.

The [OonepoleTrack](OonepoleTrack.html) object does the same thing as
**Oonepole** except that it 'tracks' changes to the state of the filter
and only performs computations if the cutoff frequency or lag time of
the filter has changed. More generally, the older functions
[reson](reson.html) and [resonz](resonz.html) do similar kinds of
filtering operations. See also the [Oequalizer](Oequalizer.html) object
for filtering capabilities.

-----

### Constructors

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
