---
title: setline()
layout: ref
---

### setline

*draw arbitrary curve of straight-line segments into an array*  
  

### Synopsis

### Description

**setline()** will store an arbitrary curve of straight-line segments in
the specified array. arglist is a list of arguments where *arglist\[0\],
arglist\[2\], arglist\[4\],* etc. are referenced times, and
*arglist\[1\],\[3\],\[5\],* etc., are ampltudes at those time. Straight
lines will be interpolated between referenced amplitudes. An
instantaneous change may be made by having successive references to the
same time, with different amplitudes. The entire curve will be squeezed
to length locations in array so that the referenced times are in effect
proportional. *n\_args* is the number of arguments contained in arglist.
This is useful for use with **table** or **tablei** to access values in
the table

### See Also

[table](table.html), [tablei](table.html).
