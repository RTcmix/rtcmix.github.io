---
title: rtgetin()
layout: ref
---

### rtgetin

*INSTRUMENT design -- sample input-reading function*  
  
  

*\[NOTE: Much of the underlying functionality of the* **rtgetin()**
*function has been superceded by the* **[Ortgetin](Ortgetin.html)**
*object.\]*

  
  
The **rtgetin()** function is used in RTcmix instrument design to read a
buffer of samples from the input stream. This will probably be a
soundfile or a real-time input source, depending on how the
[rtinput](../scorefile/rtinput.html) score directive is set. The point
where **rtgetin()** actually starts reading also depends on the
[rtsetinput](rtsetinput.html) function probably used in the
*INSTRUMENT::init()* member function.

**rtgetin()** differs from [rtaddout](rtaddout.html) in that it reads a
block of samples instead of a single "frame" (1-sample-step \*
number-of-output-channels) of samples. The buffer used by **rtgetin()**
has to be dimensioned large enough to hold the number of samples that
will be buffered by RTcmix. Since this depends on how the buffers were
set in the [rtsetparams](../scorefile/rtsetparams.html) score directive,
this usually needs to be allocated dynamically.

Here's an example of how to do this:

Where does the variable *RTBUFSAMPS* come from? It is a class variables
that is part of the general [Instrument](Instrument.html) object, used
as the base class to create user-written instruments. The
*framesToRun()* and *inputChannels* are inline access functions, also
part of the *Instrument* object, that return the *Instrument* class
variables *inputchans* and *chunksamps*.

Because **rtgetin()** reads an entire buffer, this buffer needs to be
'stepped through' inside the *INSTRUMENT::run()* method. The step size
depends on how many channels (*inputChannels()*, or *inputchans*) are in
the input stream, as samples are interleaved in the buffer. This means,
for example, that a stereo input stream will set up the buffer-array
with *left0, right0, left1, right1, left2, right2 ... leftN, rightN*.

Generally **rtgetin** is used in the *INSTRUMENT::run()* member
function, just before the sample-computing loop. It can also be used to
read in a buffer of samples from an input file at any point inside an
INSTRUMENT. **rtgetin** assumes that [rtsetinput](rtsetinput.html) was
called previously.

This function replaces the older [GETIN](GETIN.html) macro used in
disk-only cmix.

-----

### Usage

-----

### Examples

  

-----

### See Also

  - [rtsetinput](rtsetinput.html)
  - [rtgetin](rtgetin.html)
