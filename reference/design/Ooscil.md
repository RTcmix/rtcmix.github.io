---
title: Ooscil()
layout: ref
---

### Ooscil/Ooscili

*INSTRUMENT design -- wavetable and function-table oscillator objects*  
  
The **Ooscili** object is an interpolating wavetable and function table
unit generator used in RTcmix instrument design. What the heck does this
mean? It's an oscillator. It oscillates. The thing that it oscillates is
the wavetable or function table array. The slightly less-functional
**Ooscil** object does pretty much the same thing except that it does
not interpolate (except for the **nexti** method intended for use in
special cases).

They can replace the older [oscili](boscili.html) and
[oscil](oscil.html) functions used in previous incarnations of
RTcmix/cmix. They can also partially replace the [tablei](table.html)
and [table](table.html) and array/table functions used for generating
longer-span control envelopes. This cute trick is accomplished by
creating an **Ooscili** or **Ooscil** object with the frequency set to
*1.0/dur* where *dur* is the time-span of the control function.

The values returned by the **Ooscili::next()** method will be
interpolated between points in the original wavetable/function table
array. The wavetable/function table array will be accessed cyclically at
a rate determined by the frequency set for the **Ooscili** object.
Again, **Ooscil** does the same thing but no interpolation.

-----

### Constructors

-----

### Access Methods

-----

### Examples

used as an oscillator:

  
used as an envelope or control signal generator:
