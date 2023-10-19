---
title: resonz()
layout: ref
---

### resonz/rszset

*simple biquad IIR filter and initialization*  
  
*\[note: This is older code, although it should work fine. There are
many more filter programs to be found in the "objlib" (in the insts.jg
package) and "stklib" (in the insts.stk package) that implement a much
wider range of filter types. Also see the instruments based on these
filters, such as [BUTTER](../instruments/BUTTER.html),
[MOOGVCF](../instruments/MOOGVCF.html), [ELL](../instruments/ELL.html)
and [MBLOWBOTL](../instruments/MBLOWBOTL.html). The
[Oreson](Oreson.html), [Oonepole](Oonepole.html) and
[Oequalizer](Oequalizer.html) objects may also be useful.\]*

### Synopsis

\#include \<ugens.h\>  
  
float resonz(sample,array)  
float sample, \*array;  
  
rszset(cf,bw,xinit,q)  
float cf,bw,xinit,\*q;  

### Description

**resonz()** is a simple biquad IIR filter. It is initialized by a call
to **rszset()** in which the arguments are: *cf*, the center frequency,
in hz; *bw*, the distance in hz, between the half-power points on the
bell-shaped curve; the value *xinit* which if 0, cases the initial
conditions of the filter to be set to 0 (this should be set to 1 when
reinitializing the filter in the midst of performance), and *q* which is
a bookkeeping array of 5 locations.

### See Also

[reson/rsnset](reson.html)
