---
title: sffixsize()
layout: ref
---

**sffixsize** - update soundfile header's idea of sound duration

-----

### Synopsis

**sffixsize** *filename* \[*filename*...\]

-----

### Description

**sffixsize** updates the soundfile header to reflect the actual amount
of sound data the file contains. This may not have been written
correctly if the writing process ended prematurely. **sffixsize** can
fix this problem so that you can play the file, etc.

Without a *filename* argument, **sffixsize** just prints a help summary.

-----

### See Also

[sfprint](sfprint.html), [sfcreate](sfcreate.html),
[sfhedit](sfhedit.html), [sfshrink](sfshrink.html)

-----

### Authors

John Gibson \<johgibso at indiana edu\>
