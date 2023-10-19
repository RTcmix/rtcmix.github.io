---
title: hcomb()
layout: ref
---

### hcomb

*interpolating comb filter*  
  

### Synopsis

### Description

**hcomb()** is a feedback loop which will store and feedback the signal
continuously. Older inputs will decay over a specified reverberation
time. Any frequencies in the input signal whose length in samples is
close to an integral number of the number of samples in the loop will be
resonated since there will be less cancellation. This represents an
improvement over [comb](comb.html) in that the quantization of looptime
as it becomes shorter due to the integer sample length of the loop is
compensated for so that it may be precisely tuned, even up to Nyquist.
This loop must first be initialized with a call to **hcombset()**. which
intializes an array for use with an **hcomb()** filter. Note that this
is a fortran sub- routine and thus must contain arguments passed by
address, and the underscore following its name.

*loopt* is equal to 1/hz and *array* must be dimensioned at (loopt \* sr
+ 9).
