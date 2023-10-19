---
title: endnote()
layout: ref
---

### endnote

*older disk-based cmix instruments*

*\[note: this function is not superceded by anything in RTcmix, as it is
no longer needed.\]*  
  
Here is the original documentation:

-----

### Synopsis

### Description

**Endnote** merely writes what it thinks is the current overall peak
amplitude of the soundfile in the soundfile descriptor, and reports on
the execution time of the note (usr + sys) clocked from the last call to
[setnote](setnote.html), its starting and ending times, the overall peak
amplitude so far, and the peak amplitude of each output channel. Its
only really important functions are to load the peak amplitude (this can
be done by hand if the run is ended prematurely (see
[sfhedit](../standalone/sfhedit.html)), and write out any incomplete
buffer. If endnote is not called chances are that you will not get the
last sound segment. If [WIPEOUT](WIPEOUT.html) was used to write samples
the unused portion of the last buffer will be flushed with 0's.

The value **'filenum'** is the number of the file to which you have been
writing as determined by the second argument of an earlier 'open'
command.

### See Also

[WIPEOUT](WIPEOUT.html), [ADDOUT](ADDOUT.html), [LAYOUT](LAYOUT.html),
[setnote](setnote.html) [sfhedit](../standalone/sfhedit.html)

### Diagnostics

This call does not return any values. If 'filenum' does not point to an
opened writeable file the run will be terminated.
