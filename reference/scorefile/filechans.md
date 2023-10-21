---
title: filechans()
layout: ref
---

## filechans

Return soundfile channel information.

-----

### Synopsis

nchans = **filechans**(*"filename"*)

-----

### Description

**filechans** simply returns the number of channels of the soundfile
*filename*. *filename* may be an absolute or relative pathname to the
soundfile. This command does not require that the soundfile be
previously opened by the [rtinput](rtinput.html) command.

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
   nchannels = filechans("somesoundfile")
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filedur](filedur.html), [filepeak](filepeak.html),
[filesr](filesr.html), [rtinput](rtinput.html)
