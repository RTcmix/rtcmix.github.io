---
title: pchcps()
layout: ref
---

## pchcps

Convert frequency (Hz) to octave.pitch-class (oct.pc).

-----

### Synopsis

pchval = **pchcps**(*freq*)

-----

### Description

**pchcps** returns a corresponding "octave.pitch-class" (*oct.pc*) value
for a given frequency value (Hz). *oct.pc* is a way to use standard
"western" keyboard notes without having to look up the pitch-frequency
conversion. It works by arbitrarily assigning the octave of middle C to
8.00. Any semitone above middle C is added as a "hundredth" to the left
of the decimal point, i.e. 8.01 is the C\# just above middle C, 8.02 is
the D, 8.03 is the D\# (Eb), etc. up to 8.12, which is equivalent to
9.00. 9.01 is then the C\# one octave and a semitone abouve midddle C.

The fun thing about this notation is that you are not limited to
keyboard-notes. An octave.pitch-class specification of 7.07542389 will
select a frequency that is somewhere about half-way between the G (7.07)
and Ab (7.08) just below middle-C. The linear octave representation will
reflect this absolute frequency value. Different RTcmix instruments will
require the pitch or frequency to be specified in different ways.

NOTE: With the exception of [boost](boost.html), The RTcmix conversion
functions follow a pattern. The command isdivided into two halves, the
one closest to the argument represent the format of the argument, and
the one closest to the assignment represents the format to be returned.
For example, "cpspch" is divided into "cps" and "pch". The argument is
in oct.pc form ("pch") and the return value will be in cps ("cps").

The various format specifiers are:

```cpp
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

  - *freq*  
    Any number, floating point or integer, representing a direct
    frequency value (Hz).

-----

### Examples

```cpp
   pval = pchcps(440.0)
   pval = pchcps(787.98)
```

-----

### See Also

[ampdb](ampdb.html), [boost](boost.html), [dbamp](dbamp.html),
[cpslet](cpslet.html), [cpsmidi](cpsmidi.html), [cpsoct](cpsoct.html),
[cpspch](cpspch.html), [midipch](midipch.html), [octcps](octcps.html),
[octlet](octlet.html), [octmidi](octmidi.html), [octpch](octpch.html),
[pchlet](pchlet.html), [pchmidi](pchmidi.html), [pchoct](pchoct.html)
