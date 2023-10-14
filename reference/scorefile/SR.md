---
title: SR()
layout: ref
---

## SR

Return sampling rate information.

-----

### Synopsis

samprate = **SR**()

-----

### Description

**SR** returns the sampling rate of the most recently opened soundfile
by the [rtinput](rtinput.html) command, or the sampling rate of the
input audio device.

-----

### Examples

``` 
   rtinput("somesoundfile")
   srate = SR()
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html),
[filechans](filechans.html), [filedur](filedur.html),
[filepeak](filepeak.html), [filesr](filesr.html),
[rtinput](rtinput.html)
