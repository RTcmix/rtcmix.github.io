---
title: pvgetframefreqs()
layout: ref
---

## pvgetframefreqs

Retrieve the frequency bin values for a given frame in the current **PVOC** data file

-----

### Synopsis

pchvals = **pvgetframefreqs**(*frame\_number*)

-----

### Description

**pvgetframefreqs** will retrieve a list (array) of frequency (hertz) values for a specific frame in a **PVOC** analysis file, as created by the
[MiXViews](https://music.columbia.edu/~doug/MixViews/MiXViews.html)
program.  There will be half as many frequency values as there are points in the **PVOC** analysis.

-----

### Arguments

- *frame\_number*

	The frame to return.  This frame number may be fractional; **pvgetframefreqs** will interpolate the frequency values using the frames on either side.  The elapsed time represented by the frame can be calculated by dividing it by the frame rate of the file (see [pvgetframerate](pvgetframerate.html)).

### Examples

```cpp
total_frames = pvinput("some/pvocfile.pv");  // open a PVOC file
frame_to_read = total_frames / 2;		// exactly halfway
pitcharray = pvgetframefreqs(frame_to_read)
for (i = 0; i < len(pitcharray); ++i) {
	printf("PVOC Pitch[%d] is %f CPS\n", i, pitcharray[i]);
}
```

### See Also

[pvinput](pvinput.html), [pvgetframeamps](pvgetframeamps.html), [pvgetframerate](pvgetframerate.html), [PVOC](../instruments/PVOC.html)

