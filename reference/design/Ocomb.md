---
title: Ocomb()
layout: ref
---

### Ocomb/Ocombi

*INSTRUMENT design -- comb-filter (delay) objects*  
  
The **Ocomb** and **Ocombi** objects are used to build feedback delay
lines known as "comb" filters. In fact, the **Ocomb** and **Ocombi**
objects are essentially just wrappers for the [Odelay](Odelay.html) and
[Odelayi](Odelay.html) objects. However, the parameters used to set and
control the **Ocomb** and **Ocombi** objects are more convenient for
many computer-music applications. Comb filter delays are used
extensively in room-simulation and reverberation algorihms, and the
effect has also figured prominently in many 'classic computer music
pieces.

**Ocomb** is non-interpolating, so that requests for delay times that
translate into a fractional point between two samples will be 'rounded'
to the nearest sample value. This fractional delaying can happen quite
often with dynamically-changing delay lines, which makes **Ocombi**
probably a better choice in those cases. Shifting the delay time of
**Ocomb** may result in audio "glitches" because of this round-off
error, but these may be a desirable effect. **Ocombi** is slightly less
efficient because of the interpolation math to calculate a fractional
sample-point.

**Ocomb** and **Ocombi** replace the older CMIX [comb](comb.html) and
[combset](combset.html) functions.

-----

### Constructors

-----

### Access Methods

  

-----

### Examples
