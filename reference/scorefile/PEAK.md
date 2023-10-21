---
title: PEAK()
layout: ref
---

## PEAK / RIGHT\_PEAK / LEFT\_PEAK

Return amplitude peak information.

-----

### Synopsis

val = **PEAK**()  
val = **LEFT\_PEAK**()  
val = **RIGHT\_PEAK**()  
val = **PEAK**(*start, end*)  
val = **LEFT\_PEAK**(*start, end*)  
val = **RIGHT\_PEAK**(*start, end*)

-----

### Description

These commands operate upon the most recently opened input soundfile by
the [rtinput](rtinput.html) command. **PEAK** returns the overall peak
for both channels, **RIGHT\_PEAK** and **LEFT\_PEAK** return the peak
amplitudes for the left (channel 0) and right (channel 1) channels,
respectively. The optional *start* and *end* parameters are starting and
ending times to scan the soundfile (in seconds).

These routines will attempt to read the peak amplitude(s) stored in the
soundfile header. If this is not possible, then the soundfile itself
will be scanned. They do not work on a real-time audio input device.

-----

### Arguments

  - *start*  
    The number of seconds to skip before starting the peak amplitude
    scan

  - *end*  
    The endpoint (in seconds) to stop the peak amplitude scan

-----

### Examples

``` 
   rtinput("somesoundfile")
   peakval = PEAK()

   rtinput("someothersoundfile")
   lpeakval = LEFT_PEAK(3.4, 7.8)
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html),
[filepeak](filepeak.html), [filesr](filesr.html),
[rtinput](rtinput.html)
