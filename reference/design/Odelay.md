---
title: Odelay()
layout: ref
---

### Odelay/Odelayi

*INSTRUMENT design -- general delay-line objects*  
  
The **Odelay** and **Odelayi** objects are used to create and use
delay-lines for samples. **Odelay** is non-interpolating, so that
requests for delays that translate into a fractional point between two
samples will be 'rounded' to the nearest sample value. This fractional
delaying can happen quite often with dynamically-changing delay lines,
which makes **Odelayi** a better choice in those cases. **Odelayi** is
slightly less efficient because of the interpolation math to calculate a
fractional sample-point.

There are two different ways of using the **Odelay/Odelayi** objects.
The first is based on the older RTcmix delay techniques using the
[delset/delput/delget/dliget](delget.html) approach, where delayed
samples are retrieved from the delay line based upon a time- or
sample-amount of delay specified in the corresponding **delget** or
**dliget** function call. Samples are put 'into' the delay line using
**delput**. Because the delay amount is determined when samples are
retreived from the delay line, multiple taps (i.e. multiple calls to the
sample-retrieving function) are allowed for each sample-generating
cycle.

The second way of using the **Odelay/Odelayi** objects is based on
approach used in the [Synthesis ToolKit (STK)](https://ccrma.stanford.edu/software/stk) delay line
implementation (DLineL). Samples are placed into and retrieved from the
delay line simultaneously, with the length of the delay determined by a
separate function call. It is prabably best not to mix the two different
approaches, because the delay line pointer-updating is handled slightly
differently in each case.

-----

### Constructors

-----

### Access Methods

-----

### Examples
