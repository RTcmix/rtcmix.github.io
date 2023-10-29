---
title: lpcgetamps()
layout: ref
---

## lpcgetamps

Return a list of amplitude values from a **LPC** (Linear Predictive Coding) analysis data file.

-----

### Synopsis

ampvals = **lpcgetamps**(*"lpc\_analysis\_filename", npoles\_guess, firstFrame, lastFrame [,error\_threshold]*)

-----

### Description

**lpcgetamps** will retrieve a list (array) of amplitude values from an **LPC** analysis file,
most likely created by the
[MiXViews](http://music.columbia.edu/~doug/MixViews/MiXViews.html)
program.  Any subset of the total frames may be retrieved as well as all of them.

-----

### Arguments

- *"lpc\_analysis\_filename"*  
      
    The path to the **LPC** analysis file. This may be an absolute or relative. 
    If it is a relative pathname, it will be
    relative to the directory in which the CMIX command was invoked.

- *npoles\_guess*

	The number of poles in the **LPC** data.  Pass 0 unless you are reading from a "raw" (headerless) data file (very unlikely).

- *firstFrame*

	The index to the first desired frame (0 being the first).
	
- *lastFrame*

	The index to the last desired frame (anything larger than the file's frame count will be truncated to count-1).
	
- *error\_threshold*

	This allows you to effectively ignore **LPC** data frames whose error value exceeds this threshold (between 0.0 and 1.0, inclusive).  Amplitude frames for these frames will be set to zero.  This parameter is optional.

### Examples

```cpp
amparray = lpcgetamps("/some/LPC/analysis_file.lpc", 0, 0, 20)
for (i = 0; i < 20; ++i) {
	printf("Amplitude[%d] is %f\n", i, amparray[i]);
}
```

### See Also

[lpcgetpitches](lpcgetpitches.html), [LPCPLAY](../instruments/LPCPLAY.html)

