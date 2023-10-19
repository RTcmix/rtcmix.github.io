---
title: Ortgetin()
layout: ref
---

### Ortgetin

*INSTRUMENT design -- read samples from the input device or file*  
  
The **Ortgetin** object fills an input array with samples from a
soundfile or an input device, depending upon the parameters set in the
[rtinput](../scorefile/rtinput.html) scorefile command. It essentially
wraps the [rtgetin](rtgetin.html) function and simplifies the necessary
array allocation and sample-retrieval chores.

It is still the responsibility of the RTcmix instrument designer to
properly handle differing numbers of input channels (i.e. mono vs.
stereo) in the *Instrument::run()* method. **Ortgetin** will configure
it's input buffers and returned sample array based upon the values of
the
[RTBUFSAMPS](Instrument.html#inputChannels%3EinputChannels\(\)%3C/a%3E%0Afuntion%20and%20the%0A%3Ca%20href=)
<span>parameter. **Ortgetin** will automatically read a new input buffer
whenever the current buffer of input samples is exhausted. Note that
**Ortgetin** assumes that the input has been properly set using
the</span> [rtsetinput](rtsetinput.html) function.

See the [instrument design](../../tutorials/instrumentdesign.html)
tutorial for more information on how **Ortgetin** is used.

-----

### Constructors

-----

### Access Methods

-----

### Examples
