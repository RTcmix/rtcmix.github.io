---
title: rtaddout()
layout: ref
---

### rtaddout

*INSTRUMENT design -- sample output-writing function*  
  
The **rtaddout()** function is used in RTcmix instrument design to write
one frame of generated samples to the output buffer for hearing or
soundfile-writing. It will write the appropriate number of channels
depending on the value of the INSTRUMENT variable *outputchans* (see the
[Instrument](Instrument.html) listing for more information about
INSTRUMENT variables).

Typically **rtaddout** is used in the sample-computing loop inside the
*INSTRUMENT::run()* member function. **rtaddout** assumes that
[rtsetoutput()](rtsetoutput.html) was called in the *INSTRUMENT::init()*
member function.

It replaces the older [ADDOUT](ADDOUT.html) macro used in disk-only
cmix.

-----

### Usage

**int rtaddout(***float frame\_array\[\]***)**  
  

<u>*frame\_array*</u> is the name of a float array used to store a frame
of samples. A "frame" contains samples for each output channel, thus
*frame\_array* will have only one element for mono output, two for
stereo, four for quad, etc.

For example, if the output were stereo, *frame\_array* would be declared
like this:

where *frame\_array\[0\]* would contain the sample for the left output
channel, and *frame\_array\[1\]* would contain the right output channel
sample.  
  
The **rtaddout()** function returns the number of channels of the
output.

-----

### Examples

  

-----

### See Also

  - [rtsetoutput](rtsetoutput.html)
  - [rtgetin](rtgetin.html)
