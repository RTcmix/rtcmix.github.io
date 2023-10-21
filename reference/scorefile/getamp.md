---
title: getamp()
layout: ref
---

## getamp

Return amplitude value from LPC analysis data file.

-----

### Synopsis

ampval = **getamp**(*"lpc\_analysis\_file", frame\_number*)

*(note: This command appears to be broken. BGG, 7/2012)*

-----

### Description

**getamp** will retrieve an RMS amplitude value from an LPC analysis
file, possibly created by the
[MiXViews](http://music.columbia.edu/~doug/MixViews/MiXViews.html)
program. *"lpc\_data\_file"* is a string with the name of the file. This
may be an absolute or relative pathname to the LPC analysis file.
*frame\#* is the analysis frame to be retrieved.

-----

### Arguments

  - *"lpc\_analysis\_file"*  
      
    The LPC analysis file. This may be an absolute or relative pathname
    to the LPC analysis file. If it is a relative pathname, it will be
    relative to the directory in which the CMIX command was invoked.

  - *frame\_number*  
      
    The frame from which the RMS amplitude should be read. The time in
    the file will be dependent upon the frame-rate used when the LPC
    analysis was done.

-----

### Examples

``` 
   ampval = getamp("/some/LPC/analysis_file.lpc", 268)
```

-----

### See Also

[getpch](getpch.html), [LPCPLAY](../instruments/LPCPLAY.html)
