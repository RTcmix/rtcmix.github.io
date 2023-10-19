---
title: pluck()
layout: ref
---

### pluck

*Karplus-Strong ("plucked string") algorithm generator*  
  
This implements a simple Karplus-Strong filter/generator. See the
documentation for [hpluck](hpluck.html) for more information (and a
better implementation). This is initialized by the
[pluckset](pluckset.html) function.

From the source code:

-----

float pluck(float sig, float \*q)

-----

### See Also

[pluckset](pluckset.html)

The source code for the [STRUM](../instruments/STRUM.html) instrument
contains much better plucked-string algorithms. This ugen is pretty
ancient.
