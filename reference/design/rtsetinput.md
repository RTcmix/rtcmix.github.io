---
title: rtsetinput()
layout: ref
---

### rtsetinput

*INSTRUMENT design -- input initialization and setup function*  
  
The **rtsetinput()** function is used in RTcmix instrument design to set
the start time to begin reading a soundfile (if the
[rtinput](../scorefile/rtinput.html) scorefile command set a soundfile
in the score) or the start time for becoming active (if real-time input
was specified by the [rtinput](../scorefile/rtinput.html) command in the
score).

Typically it is used in the *INSTRUMENT::init()* member function of an
instrument. It handles all scheduling tasks for real-time input, and it
can also be used to set the INSTRUMENT for reading a soundfile at the
proper time.

It replaces the older [setnote](setnote.html) function used in disk-only
cmix.

-----

### Usage

-----

### Examples

  

-----

### See Also

  - [rtsetoutput](rtsetoutput.html)
