---
title: OonepoleTrack()
layout: ref
---

### OonepoleTrack

*INSTRUMENT design -- simple 'tracking' one-pole filter object*  
  
The **OonepoleTrack** object is a subclass of the
[Oonepole](Oonepole.html) object that 'tracks' changes to the cutoff
frequency or the lag time and performs computations to update them only
when they change. This only works if the caller sticks to updating
either the cutoff frequency (using the **setfreq** method) or the lag
time (using the **setlag** method), not mixing calls to both.

See the [Oonepole](Oonepole.html) documentation for a more complete
description of this filter.

-----

### Constructors

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
