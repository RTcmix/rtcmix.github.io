---
title: Odcblock()
layout: ref
---

### Odcblock

*INSTRUMENT design -- DC-blocking filter object*  
  
Based in a design from Perry Cook and Gary Scavone's [Synthesis ToolKit
(STK)](https://ccrma.stanford.edu/software/stk), the
**Odcblock** object is a simple one-pole/one-zero filter set to remove a
DC (0 Hz) offset component in the output signal. The recursive filter
equation used for this object is:

    y[n] = x[n] - x[n-1] + 0.99*y[n-1]

where *y\[n\]* and *y\[n-1\]* are the current and previous outputs of
the equation, respectively, and *x\[n\]* and *x\[n-1\]* are the current
and previous sample inputs to the filter equation.

This object is very useful in cases where the output of a
signal-processing or synthesis algorithm may not necessarily be
symmetrical around 0.0. Many physical-modelling algorithms use this
filter.

-----

### Constructors

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
