---
title: pluckset()
layout: ref
---

### pluckset

*Karplus-Strong ("plucked string") algorithm setup*  
  
This initializes a simple Karplus-Strong filter/generator
([pluck](pluck.html)). See the documentation for [hplset](hplset.html)
for more information (and a better implementation).

From the source code:

-----

void pluckset(float xlp, float amp, float seed, float c, float \*q,
float sr)

-----

### See Also

[pluck](pluck.html)

The source code for the [STRUM](../instruments/STRUM.html) instrument
contains much better plucked-string algorithms. This ugen is as old as
the hills.
