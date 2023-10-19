---
title: oscil()
layout: ref
---

### oscil/oscili/osciln/oscilni

*wavetable oscillators*  
  

### Synopsis

### Description

The units **oscil**, and **oscili**, are, respectively, a simple
table-lookup oscillator and an interpolating version of the same. The
others **osciln** and **oscilni** are designed for frequency modulation
and will accept negative sampling incre- ments. The arguments *amp* and
*si* are the amplitude and sampling increments, The argument *farray* is
the address of the table from which to fetch values. In case this table
was created with a makegen command a call to *floc* will return this
address. The *phs* argument is passed as an address since it must be
updated continuously by the **oscil** call. Thus *phs* should be
declared as float in the calling routine, and passed to **oscil** as
*\&phs*. Note that if it is declared as *\*phs* in the calling routine
space will not actually be allocated for it and an error will result.
The various oscils return floating point values and are declared as
floats in the above mentioned include file. The argument len is the size
of the table passed to oscil and if this table was created by a call to
makegen the routine fsize will find you this quantity.

### See Also

[floc](floc.html), [fsize](fsize.html).

### Bugs

These units dutifully keep the phase pointer within range by subtracting
the length from the phase until in range, so if the phase pointer is
uninitialized, and perhaps huge, several years may pass while
.123456e+38 is reduced to less than 512, for example. Also, as mentioned
above make sure that *phs* is declared as float *phs*; not float
*\*phs*; and passed as *\&phs*.
