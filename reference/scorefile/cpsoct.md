---
title: cpsoct()
layout: ref
---

## cpsoct

Convert linear octaves to frequency (Hz).

-----

### Synopsis

freq = **cpsoct**(*linoct*)

-----

### Description

**cpsoct** returns a corresponding frequency value (Hz) for a given
linear octave value. Linear octaves are similar to octave.pitch-class
(oct.pc) notation in that 8.00 is middle C, 9.00 is the C an octave
above, etc. The difference is that the fractional part of the
specification represents a direct mapping onto the notes of the scale
between octaves. For example, in oct.pc notation the value 8.06
represents F-sharp (the tritone, 1/2 the chromatic scale between 8.00
and 9.00). This would be represented as 8.5 in linear octaves.

NOTE: With the exception of [boost](boost.html), The RTcmix conversion
functions follow a pattern. The command isdivided into two halves, the
one closest to the argument represent the format of the argument, and
the one closest to the assignment represents the format to be returned.
For example, "cpspch" is divided into "cps" and "pch". The argument is
in oct.pc form ("pch") and the return value will be in cps ("cps").

The various format specifiers are:

``` 
   amp = linear amplitude (16-bit, 0-32767)
   cps = cycles per second (Hz)
   db = decibels
   midi = MIDI note number (60 is middle C)
   oct = linear octaves (8.5 is halfway between octave 8.00 [middle C] and 9.00)
   pch = octave.pitch-class (oct.pc; 8.00 is middle C, 8.02 is D, 8.12 = 9.00 = C above middle C)
   let = note-letter specification ("C4" is middle C, "C#4" is C-sharp above middle C,
      "Gb5" is G-flat the octave above middle C octave. [see pitch-reps for more info])
```

-----

### Arguments

  - *linoct*  
    Any number, floating point or integer, representing a linear octave
    pitch value

-----

### Examples

``` 
   freq = cpsoct(8.00)
   freq = cpsoct(6.55)
```

-----

### See Also

[ampdb](ampdb.html), [boost](boost.html), [dbamp](dbamp.html),
[cpslet](cpslet.html), [cpsmidi](cpsmidi.html), [cpspch](cpspch.html),
[midipch](midipch.html), [octcps](octcps.html), [octlet](octlet.html),
[octmidi](octmidi.html), [octpch](octpch.html), [pchcps](pchcps.html),
[pchlet](pchlet.html), [pchmidi](pchmidi.html), [pchoct](pchoct.html)
