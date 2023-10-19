---
title: reson()
layout: ref
---

### reson/rsnset

*simple first-order IIR filter and initialization*  
  
*\[note: This is older code, although it should work fine. There are
many more filter programs to be found in the "objlib" (in the insts.jg
package) and "stklib" (in the insts.stk package) that implement a much
wider range of filter types. Also see the instruments based on these
filters, such as [BUTTER](../instruments/BUTTER.html),
[MOOGVCF](../instruments/MOOGVCF.html), [ELL](../instruments/ELL.html)
and [MBLOWBOTL](../instruments/MBLOWBOTL.html). The
[Oreson](Oreson.html) object encapsulates these functions in a C++
object. The [Oonepole](Oonepole.html) and [Oequalizer](Oequalizer.html)
objects may also be useful.\]*

### Synopsis

\#include \<ugens.h\>  
  
float reson(sample,array)  
float sample, \*array;  
  
rsnset(cf,bw,scl,xinit,q)  
float cf,bw,scl,xinit,\*q;  
  

### Description

**reson()** is a simple IIR filter. It is initialized by a call to
**rsnset()** in which the arguments are: *cf*, the center frequency, in
hz; *bw*, the distance in hz, between the half-power points on the
bell-shaped curve; *scl* which seems to be some sort of strange and
wonderful scale factor and is set to 1 for harmonic signals, 2 for
random inputs, and 0 to set no gain factor; the value *xinit* which if
0, cases the initial conditions of the filter to be set to 0 (this
should be set to 1 when reinitializing the filter in the midst of
performance), and 'q' which is a bookkeeping array of 5 locations.

### See Also

[resonz/rszset](resonz.html)
