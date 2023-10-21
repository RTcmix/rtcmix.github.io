---
title: filedur()
layout: ref
---

## filedur

Return soundfile duration information.

-----

### Synopsis

duration = **filedur**(*"filename"*)

-----

### Description

**filedur** simply returns the duration of the soundfile *filename*.
*filename* may be an absolute or relative pathname to the soundfile.
This command does not require that the soundfile be previously opened by
the [rtinput](rtinput.html) command.

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
   dur = filedur("somesoundfile")
```

-----

### See Also

[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filepeak](filepeak.html),
[filesr](filesr.html), [rtinput](rtinput.html)
