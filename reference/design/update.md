---
title: update()
layout: ref
---

### update

*INSTRUMENT design -- retrieve PField values during sample
computation*  
  
The **update()** function is used to read data coming from a
[PField](../interface/PField.html) variable or a table-handle (see the
[maketable](../scorefile/maketable.html) scorefile command) into an
instrument. Typically **update()** is called in the *INSTRUMENT::run()*
member function inside the sample-computation loop. It is usually placed
within a conditional structure so that it gets called at the reset rate
set by the [reset](../scorefile/reset.html) scorefile command.

**update()** is new in RTcmix4.0, and is the mechanism that allows
Instrument parameters to be altered as a note is being played. See the
source code for various instruments for examples of how this function is
employed.

-----

### Usage

**int update(***double p\[\], int nvalues***)**  
**int update(***double p\[\], int nvalues, unsigned fields***)**  
  

<u>*p\[\]*</u> is an array of doubles that will be filled with values
coming from a PField variable or table-handle. For example, if the
following scorefile invoked an Instrument note:

then any data being changed through the variable *handle* when the note
was being executed would appear as element 4 (*p\[3\]*) in the *p\[\]*
array.  
<u>*nvalues*</u> is the total number of values that will be scanned into
the *p\[\]* array. Note that *p\[\]* has to be dimensioned large enough
to accommodate *nvalues*.  
<u>*fields*</u> is an optional parameter (actually set to "0" if it
isn't present) that represents a bitmask to retrieve only specific
values into the *p\[\]* array. In the example above, if the call to
**update()** in the Instrument was:

then only *p\[3\]* would be updated. This results in an increase in
Instrument efficiency. Note that only 31 bits are possible with this
scheme, however, so that the bitmask only supports the first 31
p-fields.

These versions of **update()** always returns 0 for some reason.

  
**double update(***int index***)**  
**double update(***int index, int totframes***)**  
  

<u>*index*</u> is the index of the p-field to retrieve. If *index* is 2,
for example, then the current value for *p\[2\]* will be returned as a
double.  
<u>*totframes*</u> is an optional argument representing the total number
of sample frames over which this **update** will operate. This is useful
for tables (control envelopes) that are intended only to span a portion
of an executing note. An example of this might be a table to perform a
fade-out during the 'ring-down' portion of an Instrument employing a
feedback delay line.

To put it another way, calling this version of **update** with the
optional *totframes* argument makes the pfield span the duration
corresponding to *totframes* instead of
[nSamps()](Instrument.html#nSamps). Note that it's much more efficient
to grab all the pfields using the first update method than to use this
update method multiple times, so try to avoid this method when you can
use the other one.

  

-----

### Examples

  

-----

### See Also

  - [Instrument](Instrument.html)
