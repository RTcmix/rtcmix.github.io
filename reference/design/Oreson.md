---
title: Oreson()
layout: ref
---

### Oreson

*INSTRUMENT design -- simple first-order IIR filter object*  
  
**Oreson** duplicates the functionality of the older CMIX functions
[reson](reson.html) and [rsnset](reson.html). It builds a simple IIR
filter given a desired center frequency and bandwidth, very useful in
parallel for building complicated filter response curves (see the
[IIR](../instruments/IIR.html) instrument for an example of **Oreson**
use).

The general recursive filter equation used by this object is:

    y[n] = a*x[n] + b0*y[n-1] - b1*y[n-2]

where *y\[n\]*, *y\[n-1\]* and *y\[n-2\]* are the current and two
previous outputs of the equation, and *x\[n\]* is the current input.
*a*, *b0* and *b1* are the coefficients to the equation that determine
the characteristics of the filter, depending upon the parameters set in
the constructor. Presently the **Oreson** filter object does not allow
for dynamic changes to the filter equation.

-----

### Constructors

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
