---
title: allpass()
layout: ref
---

### allpass/comb/combset

*short feedback delay lines*  
  

### Synopsis

### Description

This is a feedback loop which will store and feedback the signal
continuously. Older inputs will decay over a specified reverberation
time. Any frequencies in the input signal whose length in samples is
close to an integral multiple of the number of samples in the loop will
be resonated since there will be less cancellation. This loop must first
be initialized with a call to **combset()**. which intializes an array
for use with a **comb()** or an **allpass()** filter.

**Loopt** is 1/hz, which is set to the 'fundamental' of the comb. The
number of samples in the loop is **loopt \* samplingrate.**

**Reverbtime** is the length of time old samples take to decay 60 db.

**init** is a flag which, if FALSE, forces the loop to be filled with
zeroes. Setting **init** to TRUE will cause the contents of the array
loop to be unaltered. Note that this will contain garbage unless it has
been previously used by another filter.

**Array** is the address of a floating point array which must be
dimensioned to 2+number\_of\_samples\_in\_loop, which is found by
loopt\*sampling\_rate.

**allpass()** is initialized in the same way as the comb filter but has
a feature which cancels the frequency response of the filter and
theoretically passes all frequencies at equal amplitude.

*\[note: Because of the integer length of the delay-line used, **comb**
will not accurately reproduce higher frequencies (the short delay-length
will 'quantize' the frequency resolution). For an accurate
high-frequency comb, use the [hcomb](hcomb.html) comb filter.\]*
