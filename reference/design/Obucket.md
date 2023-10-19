---
title: Obucket()
layout: ref
---

### Obucket

*INSTRUMENT design -- arbitrary buffering object*  
  
The **Obucket** object keeps a private buffer of floating-point numbers,
and a 'call-back' function can be registered that will be invoked when
the buffer is filled. One floating-point number at a time can be added
to the internal buffer, and when **Obucket** sees that the buffer is
full, it calls the 'call-back' function with the buffer, its length and
a user-supplied context as arguments. This is useful for instruments
that must process in blocks whose size has no clear and predictable
relationship to the RTcmix buffer size (for example, instruments that do
FFT-based processing).

-----

### Constructors

-----

### Access Methods

  

-----

### Examples
