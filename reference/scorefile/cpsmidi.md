---
title: cpsmidi()
layout: ref
---

## cpsmidi

Convert MIDI note number to frequency (Hz).

-----

### Synopsis

freq = **cpsmidi**(*midinote*)

-----

### Description

**cpsmidi** returns a corresponding frequency value (Hz) for a given
MIDI note number. **cpsmidi** will not do fractional MIDI note values.
Any fractional MIDI note number will be truncated prior to conversion
(i.e. 62.78 -\> 62).

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

  - *midinote*  
    Any postive number, floating point or integer, representing a MIDI
    note number

-----

### Examples

``` 
   freq = cpsmidi(72)
   freq = cpsmidi(60.0)
```

-----

### See Also

[ampdb](ampdb.html), [boost](boost.html), [dbamp](dbamp.html),
[cpslet](cpslet.html), [cpsoct](cpsoct.html), [cpspch](cpspch.html),
[midipch](midipch.html), [octcps](octcps.html), [octlet](octlet.html),
[octmidi](octmidi.html), [octpch](octpch.html), [pchcps](pchcps.html),
[pchlet](pchlet.html), [pchmidi](pchmidi.html), [pchoct](pchoct.html)
