---
title: pvinput()
layout: ref
---

## pvinput

Open the specified **PVOC** file and make it the current data file

-----

### Synopsis

frame\_count = **pvinput**(*"path\_to\_data\_file"*)

-----

### Description

**pvinput** opens and verifies the **PVOC** data file *path\_to\_data\_file*.  *path\_to\_data\_file* may be an absolute or relative pathname to the file.  **pvinput** returns the frame count on success, else -1.

-----

### Arguments

  - *path\_to\_data\_file*  
	A string representing the name of the **PVOC** data file to be opened. It
    may be an absolute or relative pathname to the file. If it is
    relative, then it will be relative to the directory where the CMIX
    command was invoked.

-----

### Examples

```cpp 
frame_count = pvinput("/path/to/datafile")
// check for error
if (frame_count < 0) {
	error("failed to open %s\n", "/path/to/datafile");
}
```

-----

### See Also
[pvgetbin](pvgetbin.html), [pvgetbincount](pvgetbincount.html), [pvgetframe](pvgetframe.html), [pvgetframefreqs](pvgetframefreqs.html), [pvgetframerate](pvgetframerate.html), [PVOC](../instruments/PVOC.html)
