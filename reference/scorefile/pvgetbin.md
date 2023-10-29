---
title: pvgetbin()
layout: ref
---

## pvgetbin

Retrieve the complete amplitude and frequency bin values for a given frame in the current **PVOC** data file

-----

### Synopsis

binvals = **pvgetbin**(*frame\_number*)

-----

### Description

**pvgetbin** will retrieve a list (array) containing alternating amplitude and frequency values for all bins in a specific frame in a **PVOC** analysis file, as created by the
[MiXViews](http://music.columbia.edu/~doug/MixViews/MiXViews.html)
program.  There will be as many frequency values as there are points in the **PVOC** analysis.

-----

### Arguments

- *frame\_number*

	The frame to return.  This frame number may be fractional; **pvgetbin** will interpolate the values using the frames on either side.  The elapsed time represented by the frame can be calculated by dividing it by the frame rate of the file (see [pvgetframerate](pvgetframerate.html)).

### Examples

```cpp
total_frames = pvinput("some/pvocfile.pv");  // open a PVOC file
frame_to_read = total_frames / 2;		// exactly halfway
bin_array = pvgetbin(frame_to_read)
for (i = 0; i < len(bin_array); i += 2) {
	printf("PVOC Amp: %f Pitch %f CPS\n", bin_array[i], bin_array[i+1]);
}
```

### See Also

[pvinput](pvinput.html), [pvgetframeamps](pvgetframeamps.html), [pvgetframerate](pvgetframerate.html), [PVOC](../instruments/PVOC.html)

