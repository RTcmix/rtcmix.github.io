---
title: rtbaddout()
layout: ref
---

### rtbaddout

*INSTRUMENT design -- sample output-writing function*  
  
The **rtbaddout()** function is used in RTcmix instrument design to
write an array of generated samples to the output buffer for audition or
soundfile-writing. It will write a block of samples corresponding to the
*length* argument of the function.

Typically **rtbaddout** is used inside the *INSTRUMENT::run()* member
function, but after in the sample-computing loop (i.e. an array of
samples needs to be computed). **rtbaddout** assumes that
[rtsetoutput()](rtsetoutput.html) was called in the *INSTRUMENT::init()*
member function.

It replaces the older [baddout](ADDOUT.html) function used in disk-only
cmix.

-----

### Usage

**int rtbaddout(***float sample\_array\[\], int length***)**  
  

<u>*sample\_array*</u> is the name of a float array used to store an
array of samples. This array needs to be set up properly for mono or
stereo (interleaved) output; i.e. if it is a 2-channel array, the left
and right channels should be interleaved (ch0, ch1, ch0, ch1, ...)
through the entire array.

For example, if the output were stereo, *sample\_array* for 1024 sample
would be declared like this:

where *sample\_array\[0\]* would contain the first sample for the left
output channel, and *sample\_array\[1\]* would contain the first right
output channel sample, *sample\_array\[2\]* would contain the second
left output sample, *sample\_array\[3\]* would hold the second right
output sample, etc.  
  
<u>*length*</u> is the total number of samples in the array.  
  
The **rtbaddout()** function returns the number of samples written. See
also the [rtaddout](rtaddout.html) function.

-----

### Examples

  

-----

### See Also

  - [rtsetoutput](rtsetoutput.html)
  - [rtgetin](rtgetin.html)
