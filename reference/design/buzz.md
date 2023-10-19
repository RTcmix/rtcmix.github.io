---
title: buzz()
layout: ref
---

### buzz/bbuzz

*buzz (pulse) oscillators*  
  

### Synopsis

### Description

The units **buzz,** and **bbuzz,** return a pulse-like signal in which
there are **"hn"** harmonics of equal amplitude. The peak amplitude of
the signal will be **"amp."** The argument **"hn"** must have no
fractional part. **"si"** is the sampling increment, **"f"** is the
address of a 1024 location array created with makegen and containing one
complete cycle of a sine wave. (see [floc](floc.html)) **"phs"** is the
address of a phase counter and must be initialized at 0. **bbuzz()**
will compute a block of samples with one call. The argument **"a"**,
points to an array of floats with **"alen"** arguments, which will be
filled by the call. The **"phs"** pointer will be updated accordingly.

### See Also

[floc](floc.html), [fsize](fsize.html).

### Bugs

Not much protection against uninitialized phase pointers or **hn**
arguments with fractional parts. **bbuzz()** doesn't return anything, it
just fills the array.
