---
title: filerms()
layout: ref
---

## filerms

Return soundfile peak **RMS** amplitude.

-----

### Synopsis

rms_amp = **filerms**(*"filename"*)

-----

### Description

**filerms** returns the peak **RMS** amplitude of the soundfile *filename*.
*filename* may be an absolute or relative pathname to the soundfile.
This command does not require that the soundfile be previously opened by
the [rtinput](rtinput.html) command.

**RMS** (**R**oot **M**eans **S**quared) amplitudes are a standard way of measuring sound level.  The value is calculated by squaring the value of every sample, summing those values, taking the average (that's the "means squared"), and then taking the square root of that number.

-----

### Arguments

  - *"filename"*  
    A string representing the name of the soundfile to be queried. It
    may be an absolute or relative pathname to the soundfile. If it is
    relative, then it will be relative to the directory where the CMIX
    command was invoked.

-----

### Examples

```cpp
rms_amp = filerms("somesoundfile")
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html), [filepeak](filepeak.html),
[filesr](filesr.html), [rtinput](rtinput.html)
