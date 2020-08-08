---
title: CHANS()
layout: ref
---

**CHANS** - return input soundfile channel information

-----

### Synopsis

nchans = **CHANS**()

-----

### Description

**CHANS** returns the number of channels in an input file or audio
stream opened by a previous [rtinput](rtinput.html) command.

-----

### Examples

``` 
   rtinput("somesoundfile")
   val = CHANS()
```

-----

### See Also

[DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html),
[filepeak](filepeak.html), [filesr](filesr.html),
[rtinput](rtinput.html)
