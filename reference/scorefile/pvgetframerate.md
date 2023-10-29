---
title: pvgetframerate()
layout: ref
---

## pvgetframerate

Return the frame rate for the current **PVOC** data file

-----

### Synopsis

frame_rate = **pvgetframerate**()

-----

### Description

**pvgetframerate** returns the frame rate for the current **PVOC** analysis file, as created by the
[MiXViews](http://music.columbia.edu/~doug/MixViews/MiXViews.html)
program, and opened using [pvinput](pvinput.html).  The units are frames per second.

-----

### Examples

```cpp
total_frames = pvinput("some/pvocfile.pv");  // open a PVOC file
framerate = pvgetframerate();
```

### See Also

[pvinput](pvinput.html), [pvgetframeamps](pvgetframeamps.html), [pvgetframefreqs](pvgetframefreqs.html), [pvgetframerate](pvgetframerate.html), [PVOC](../instruments/PVOC.html)

