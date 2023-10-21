---
title: DUR()
layout: ref
---

## DUR

Return soundfile duration information.

-----

### Synopsis

duration = **DUR**()

-----

### Description

**DUR** returns the duration of the most recently opened soundfile by
the [rtinput](rtinput.html) command.

-----

### Examples

```cpp
   rtinput("somesoundfile")
   val = DUR()
```

-----

### See Also

[CHANS](CHANS.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html),
[filepeak](filepeak.html), [filesr](filesr.html),
[rtinput](rtinput.html)
