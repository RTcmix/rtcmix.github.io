---
title: makeconverter()
layout: ref
---

## makeconverter

Read data from one *pfield-handle* or *table-handle* and transform it to a
different representation, passing the altered data out through another
*pfield-handle*.

-----

### Synopsis

pfield = **makeconverter**(*input\_handle*, *"converter\_type"*)

-----

### Description

**makeconverter** returns a *pfield-handle* that will deliver data by
applying an RTcmix data-representation conversion routine as specified
by the *"convertor\_type"* string argument. This will be applied to data
coming through the *input-handle* variable. The *input\_handle* variable
can be any of the PField-derived variables in RTcmix, such as a
*table-handle* (see [maketable](maketable.html)) or another
*pfield-handle* (see [makeconnection](makeconnection.html),
[makeLFO](makeLFO.html) or [makerandom](makerandom.html)).

Many of the arguments for **makeconverter** may themselves also be
pfield-handles.

-----

### Arguments

  - *input\_handle*  
      
    This is a *pfield-handle* variable referring to a data stream,
    possibly from an external device or interface or an internal PField
    data-generating process.

  - *converter\_type*  
      
    This string value (i.e., enclosed in "double quotes" in the
    scorefile) determines which RTcmix data-format conversion routines
    (listed below) will be used to transform the values coming through
    the *input\_handle*. The *pfield-handle* returned from the
    **makeconverter** command will refer to this transformed data
    stream.

-----

### Converter Types

The string argument for the converter to operate on the *input\_handle*
data stream can be any of the following:

  - *"ampdb"* &mdash; convert amp in decibels to a linear amplitude  
  - *"dbamp"* &mdash; convert linear amplitude to amp in decibels  
  - *"cpsoct"* &mdash; convert linear octaves to cycles per second  
  - *"octcps"* &mdash; convert cycles per second to linear octaves  
  - *"octpch"* &mdash; convert octave.pitch-class to linear octaves  
  - *"cpspch"* &mdash; convert octave.pitch-class to cycles per second  
  - *"pchoct"* &mdash; convert linear octaves to octave.pitch-class  
  - *"pchcps"* &mdash; convert cycles per second to octave.pitch-class  
  - *"pchmidi"* &mdash; convert MIDI note number to octave.pitch-class  
  - *"octmidi"* &mdash; convert MIDI note number to linear octaves  
  - *"midipch"* &mdash; convert octave.pitch-class to MIDI note number  
  - *"boost"* &mdash; converts a pan value (0-1) to a linear amplitude that,
    when multiplied by the amplitude of a note, achieves constant-power
    panning (compensating for the "hole in the middle")

For more information on these conversion types, see the documentation
for the various RTcmix data format converters (such as
[ambdb](ampdb.html), [cpsoct](cpsoct.html), [pchmidi](pchmidi.html),
etc. (See the [SEE ALSO](#see%20also) section below.)

-----

### Examples

``` 
   mpfield = makeconnection("midi", 1, 127, 60, 0, 1, "noteonpitch")
   octpfield = makeconverter(mpfield, "pchmidi")
```

*mpfield* receives MIDI note number data from MIDI NoteOn events, and the
**makeconverter** command will change them into the octave.pitch-class
(see [pchcps](pchcps.html) for a description of octave.pitch-class)
notation format used by many RTcmix Instruments. The *octpfield*
variable could then be used to control Instrument pitch dynamically.

-----

### See Also

[maketable](maketable.html), [makefilter](makeconnection.html),
[makeLFO](makeLFO.html), [makerandom](makerandom.html),
[makefilter](makefilter.html), [makemonitor](makemonitor.html),
[ampdb](ampdb.html), [boost](boost.html), [dbamp](dbamp.html),
[cpsmidi](cpsmidi.html), [cpslet](cpslet.html), [cpsoct](cpsoct.html),
[cpspch](cpspch.html), [midipch](midipch.html), [octcps](octcps.html),
[octlet](octlet.html), [octmidi](octmidi.html), [octpch](octpch.html),
[pchcps](pchcps.html), [pchlet](pchlet.html), [pchmidi](pchmidi.html),
[pchoct](pchoct.html)
