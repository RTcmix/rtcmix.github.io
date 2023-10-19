---
title: rtsetoutput()
layout: ref
---

### rtsetoutput

*INSTRUMENT design -- output initialization and setup function*  
  
The **rtsetoutput()** function is used in RTcmix instrument design to
set the start time and duration of a note. Typically it is used in the
*INSTRUMENT::init()* member function of an instrument. It handles all
scheduling tasks for real-time output, and it also will set the note up
for writing into a soundfile (if the
[rtoutput](../scorefile/rtoutput.html) scorefile command was specified
in the score) at the proper time.

It replaces the older [setnote](setnote.html) function used in disk-only
cmix.

-----

### Usage

-----

### Examples

  

-----

### See Also

  - [rtsetinput](rtsetinput.html)
