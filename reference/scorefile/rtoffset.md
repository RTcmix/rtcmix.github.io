---
title: rtoffset()
layout: ref
---

## rtoffset

Run the score in "fast forward" to a time and then begin playback.

-----

### Synopsis

**rtoffset**(*offset\_time*)

-----

### Description

**rtoffset** runs the current score in "fast forward", which means that everything in the score runs and samples are generated, but the operation is done as fast as possible with no sound playback.  This allows the possibility of auditioning a moment far into a long score without the need to wait through all the proceeds it.

-----

### Arguments

  - *offset\_time*  
    The time in seconds to fast forward to in the score.

-----

### Examples

```cpp
rtsetparams(44100, 2)
rtinput("some/audio/file.wav");
file_duration = DUR();
MIX(0, 0, file_duration, 1, 0, 1);

// This will cause the playback to run the score silently up to the
// requested time, and then the second half of the file will play.
rtoffset(file_duration/2)
```
-----

### See Also
[Minc](Minc.html), [rtsetparams](rtsetparams.html)