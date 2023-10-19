---
title: setnote()
layout: ref
---

### setnote

*older disk-based cmix instruments*

superceded by [rtsetoutput](rtsetoutput.html) and and
[rtsetinput](rtsetinput.html).  
  
Here is the original documentation:

-----

### Synopsis

### Description

**setnote()** positions the disk pointers on the previously opened file
number to compute (duration \* sampling rate). If **starttime** is a
negative value its absolute value is equal to the number of samples per
channel to skip on the file before reading or writing, and if
**duration** is negative its value equals the number of samples per
channel to compute. If **setnote()** seeks beyond the current end of the
file empty space will be created between that point and the position it
points to. This will appear to all software as 0's but will not actually
hog up any real disk space. The D-A conversion program will play silence
during that point.

### See Also

[ADDOUT](ADDOUT.html), [WIPEOUT](WIPEOUT.html), [LAYOUT](LAYOUT.html),
[endnote](endnote.html)

### Diagnostics

If something is wrong **setnote()** will kill the run by itself.
