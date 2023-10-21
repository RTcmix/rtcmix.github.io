---
title: boost()
layout: ref
---

## boost

Convert linear pan amplitude to *constant power* pan amplitude.

-----

### Synopsis

powerpan = **boost**(*linpan*)

-----

### Description

**boost** accepts a pan value (0.0-1.0) and returns the amplitude
scaling factor necessary to avoid a "hole in the middle" for a panned
note. In other words, for static-panned notes, it lets you convert the
linear panning law used by most instruments into a constant-power pan
with sqrt taper.

Examples of values returned by **boost**:

```cpp
   boost(0) => 1
   boost(1) => 1
   boost(0.25) => 1.265
   boost(0.5) => 1.414
```

-----

### Arguments

  - *linpan*  
    A pan value (0.0 - 1.0)

-----

### Examples

```cpp 
   pan = 0.53
   powcorrection = boost(pan)
   STEREO(0, 0, 3.4, 1.0 * powcorrection, pan)
```

-----

### See Also

[ampdb](ampdb.html), [dbamp](dbamp.html), [cpsmidi](cpsmidi.html),
[cpslet](cpslet.html), [cpsoct](cpsoct.html), [cpspch](cpspch.html),
[midipch](midipch.html), [octcps](octcps.html), [octlet](octlet.html),
[octmidi](octmidi.html), [octpch](octpch.html), [pchcps](pchcps.html),
[pchlet](pchlet.html), [pchmidi](pchmidi.html), [pchoct](pchoct.html)
