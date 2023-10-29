---
title: filedc()
layout: ref
---

## filedc

Return the averaged DC level in a soundfile.

-----

### Synopsis

dclevel = **filedc**(*"filename"*)

-----

### Description

**filedc** returns the averaged DC level of the soundfile *filename*.
*filename* may be an absolute or relative pathname to the soundfile.
This command does not require that the soundfile be previously opened by
the [rtinput](rtinput.html) command.

The DC (originally from "Direct Current") level represents the total averaged amount of zero offset is present in the waveform.  A perfectly symmetrical sine wave would return 0.0.  The farther positive or negative the value, the more "skewed" off zero the waveform is.

-----

### Arguments

  - *"filename"*  
A string representing the name of the soundfile to be queried. It may be an absolute or relative pathname to the soundfile. If it is relative, then it will be relative to the directory where the CMIX command was invoked.

-----

### Examples

```cpp 
dc_offset = filedc("somesoundfile")
```

-----

### See Also
[CHANS](CHANS.html), [DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filepeak](filepeak.html),
[filesr](filesr.html), [rtinput](rtinput.html), [DCBLOCK](../instruments/DCBLOCK.html)

