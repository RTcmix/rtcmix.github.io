---
title: cpslet()
layout: ref
---

## cpslet

Convert text-string note letter representation to frequency (Hz).

-----

### Synopsis

freq = **cpslet**(*"lettername"*)

-----

### Description

**cpslet** returns a corresponding frequency value (Hz) for a
text-string note letter representation. In this representation, the note
letter-name is given with a capital letter ("A", "B", "C", etc.)
followed by an optional "\#" (sharp) or "b" (flat) modifier, and then an
octave-specifier (octave "4" is middle C). See
[pitch-reps](http://www.musiccog.ohio-state.edu/Humdrum/representations/pitch.rep.html)
for more information about this method of represetation.

If the pitch-specification is invalid or malformed, **cpslet** will
return the value for middle C (261.625565301 Hz).

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
      "Gb5" is G-flat the octave above middle C octave. [again, see pitch-reps for more info])
```

-----

### Arguments

  - *"lettername"*  
    A text-string representing a valid note name (see above)

-----

### Examples

``` 
   freq = cpslet("F2")
   freq = cpslet("A#5")
```

-----

### See Also

[ampdb](ampdb.html), [boost](boost.html), [dbamp](dbamp.html),
[cpsmidi](cpsmidi.html), [cpsoct](cpsoct.html), [cpspch](cpspch.html),
[midipch](midipch.html), [octcps](octcps.html), [octlet](octlet.html),
[octmidi](octmidi.html), [octpch](octpch.html), [pchcps](pchcps.html),
[pchlet](pchlet.html), [pchmidi](pchmidi.html), [pchoct](pchoct.html)
