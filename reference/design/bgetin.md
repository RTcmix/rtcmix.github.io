---
title: bgetin()
layout: ref
---

### bgetin/blayout/baddout/bwipeout

*older disk-based cmix instruments*

superceded by [rtgetin](rtgetin.html), [rtaddout](rtaddout.html) and
[rtbaddout](rtbaddout.html) and  
  
Here is the original documentation:

-----

### Name

### Synopsis

### Description

These routines will read or write a block of samples from or to the
disk. The arrays **'input'** and **'output'** are in floating point
form, even if the disk file is in 16-bit integer form. **'fno'** is the
Cmix unit number for the file being accessed, and **'size'** is the
total number of samples in the array. Note that **'size'** is NOT the
number of samples per channel, but rather the total number of samples.
The routines act like their sample-by-sample namesakes: **blayout()**
and **LAYOUT(),** **baddout()** and **ADDOUT(), bwipeout()** and
**WIPEOUT().** They do not return a value, however, but rather the named
array, loaded with samples, kicked up by **'size'** from the last call.
As with the other routines, they are positioned by
[setnote()](setnote.html) and cleaned up by [endnote.](endnote.html)
They are considerably faster than these units, however, and should be
used when optimizing code, perhaps in conjunction with the [block unit
generators.](blockugens.html)

### See Also

[I/O](open.html), [setnote](setnote.html), [endnote](endnote.html),
[blockugens.](blockugens.html)
