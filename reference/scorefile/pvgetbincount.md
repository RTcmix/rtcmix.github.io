---
title: pvgetbincount()
layout: ref
---

## pvgetbincount

Return the total number of bin slots in the current **PVOC** data file

-----

### Synopsis

total_bins = **pvgetbincount**()

-----

### Description

**pvgetbincount** returns the total number of bin slots in the current **PVOC** analysis file, as created by the
[MiXViews](https://music.columbia.edu/~doug/MixViews/MiXViews.html)
program, and opened using [pvinput](pvinput.html).  This will represent the total amplitude bins plus the total frequency bins.

-----

### Examples

```cpp
total_frames = pvinput("some/pvocfile.pv");  // open a PVOC file
total_bins = pvgetbincount();
```

### See Also

[pvinput](pvinput.html), [pvgetframeamps](pvgetframeamps.html), [pvgetframefreqs](pvgetframefreqs.html), [pvgetframerate](pvgetframerate.html), [PVOC](../instruments/PVOC.html)

