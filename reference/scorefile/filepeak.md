---
title: filepeak()
layout: ref
---

## filepeak

Return soundfile peak amplitude information.

-----

### Synopsis

peakval = **filepeak**(*"filename"*\[, *starttime*\[, *endtime*\[,
*chan*\]\]\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**filepeak** returns the peak amplitude value for the soundfile
*filename*. *filename* may be an absolute or relative pathname to the
soundfile. This command does not require that the soundfile be
previously opened by the [rtinput](rtinput.html) command.

The optional *starttime* parameter can be used to set a starting point
to scan for the peak amplitude. Similarly, the *endtime* optional
parameter can set an endpoint for the scan. The optional *chan*
parameter may be used to specify which channel of a multi-channel
soundfile to scan, otherwise the peak among all channels will be
returned.

-----

### Arguments

  - *"filename"*  
    A string representing the name of the soundfile to be queried. It
    may be an absolute or relative pathname to the soundfile. If it is
    relative, then it will be relative to the directory where the CMIX
    command was invoked.

  - *starttime*  
    An optional parameter specifying the starting point (in seconds) to
    begin the scan for a peak amplitude.

  - *endtime*  
    An optional parameter specifying the ending point (in seconds) to
    begin the scan for a peak amplitude.

  - *chan*  
    An optional parameter specifying which channel to scan for a peak
    amplitude (RTcmix begins numbering channels with "0").

-----

### Examples

``` 
   peak = filepeak("somesoundfile")
   peak = filepeak("somesoundfile", 77.8, 98.2, 1))
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html),
[filesr](filesr.html), [rtinput](rtinput.html)
