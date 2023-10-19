---
title: getsample()
layout: ref
---

### getsample/GETSAMPLE/getsetnote

*older disk-based cmix instruments*

superceded by [rtgetin](rtgetin.html) and
[rtsetinput](rtsetinput.html).  
  
Here is the original documentation:

-----

### Synopsis

### Description

**getsample** will fetch a block of samples (one for each channel) at
sample number **samplenum** from a soundfile. If **samplenum** has a
fractional part, the values returned will be interpolated between
samples. The values are returned in the **c** array, channel 0 in
**c**\[0\], channel 1 in **c**\[1\] etc. The **c** array must be
dimensioned properly in the calling routine. The argument **filenum** is
the assigned number of the input file being read. **getsetnote** is the
required initialization routine and is identical in function with
[setnote](setnote.html) except that it points (\***getsample**)() to the
appropriate floating point or integer routine, depending on the nature
of the file being read, and does some additional necessary file
positioning. In ugens.h GETSAMPLE is defined as (\*getsample) so the
routine can be called analogously with [ADDOUT](ADDOUT.html),
[LAYOUT](LAYOUT.html), etc., e.g.

GETSAMPLE(samplenum,c,filenum)

### See Also

[setnote](setnote.html), [GETIN](GETIN.html), [ADDOUT](ADDOUT.html)
