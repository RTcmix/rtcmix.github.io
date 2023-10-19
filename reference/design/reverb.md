---
title: reverb()
layout: ref
---

### reverb/rvbset

*poor-quality reverberator*  
  

### Synopsis

### Description

**reverb** is a package of four combs and two allpass filters which
attempts to create the effect of diffused, reverberated sound. This
filter must first be initialized with a call to **rvbset()**. which
intializes an array for use with a **reverb()** filter.

*reverbtime* is the length of time old samples take to decay 60 db.

*init* is a flag which, if FALSE, forces the loop to be filled with
zeroes. Setting init to TRUE will cause the contents of the array loop
to be unaltered.

*array* is the address of a floating point array which must be
dimensioned to 1583 \* SR + 18. This implementation consists of four
comb and two allpass filters, and is the old MUSIC4 version. It leaves a
lot to be desired.
