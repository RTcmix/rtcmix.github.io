---
title: hpluck()
layout: ref
---

### hpluck/hplset

*Karplus-Strong ("plucked string") generator/filter and
initialization*  
  

### Synopsis

### Description

**hpluck()** is an implementation of the Karplus-Strong plucked string
algorithm described in the *Computer Music Journal*, vol. 7 no. 3, pp.
43-55. Basically it fills a table array with random numbers on
initialization and applies a low-pass filter to the table during
performance. The signal thus begins with a burst of noise and dies away
to a sine wave. This sounds remarkably liked a plucked string. This unit
must first be initialized with a call to **hplset()**. which (groan) is
still in Fortran, so the name must be followed by the underscore and all
arguments must be passed by address. The value *xlp* is the loop time
(1/hz), *dur* the expected duration of the note, *dynam*, specified in
hz is a brightness factor, *plamp* is the overall amplitude of the
result, *seed*, is a random seed value for the initial table, *sr* is
the sampling rate and *array* which must be dimensioned at least at
(9+xlp\*sr). The arguments for **hpluck()** itself are the loaded
*array* and an arbitrary input signal, which will be effected with a
comb-like output. The relation between the input signal and the pluck
can be manipulated by tinkering with the amplitude of the input signal
and plamp. If *plamp* is set to 0, the whole effect of the pluck will be
on the input signal, and if the input signal is 0, the result will be
normal plucked string synthesis. We find that since the plucked signal
dies away to dcbias that it is usually necessary to add an envelope to
avoid a thump at the end of the note. The strength of the initial pluck
can then be tinkered with by manipulating the *dur* argument, which
calculates coefficients to bring the signal to dcbias in that amount of
time.

### See Also

The source code for the [STRUM](../instruments/STRUM.html) instrument
contains much better plucked-string algorithms. This ugen is pretty
ancient (note the reference to Fortran above).
