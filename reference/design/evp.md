---
title: evp()
layout: ref
---

### evp/evset

*envelope/control function initialization and generator*  
  

### Synopsis

### Description

**evp()** is an envelope generator which can be called at arbitrary
times since it looks up its position according to the ratio between
*nsample* and the total number of expected samples. Thus it can be
called at arbitrary times, as in a control loop, without any extra
footwork. *nsample* is the number of the current sample, with the first
sample counting from 0. This is typically returned by the
[currentFrame](Instrument.html#currentFrame) function. *f1* and *f2* are
the pointers to the function arrays used for rise and decay,
respectively. These are set by [floc](floc.html). *envals* is a
5-location array of floats for bookkeeping purposes. **evset()** is the
initialization routine and asks for the*duration, rise,* and *decay*
time in seconds. If *rise* or *decay* are negative numbers they are
interpreted as a fraction of the duration. *nfrise* is the number of the
rise function. The size of the rise and decay functions must be the
same. The decay function is sampled backwards.

### See Also

[floc](floc.html), [fsize](fsize.html).
