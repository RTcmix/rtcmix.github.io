---
title: sfprint()
layout: ref
---

**sfprint** - print soundfile information

-----

### Synopsis

**sfprint** \[**-v**\] \[**-q**\] *filename* \[*filename*...\]

-----

### Description

Prints the following information about a soundfile: the header type and
data format, the sampling rate, the number of channels, the number of
bytes per sample word ("class''), the maximum amplitude value for each
channel and its location (if available), and the duration of the sound.

Without a *filename* argument, **sfprint** prints a help summary.

-----

### Options

  - **-v**  
      
    Also print the number of sample frames and the header size.
  - **-q**  
      
    Don't print anything, not even error messages. Only return status.

-----

### Returns

0 if successful; 1 if any of the file arguments is not a sound file.

-----

### Notes

The output is roughly in the same format as sfprint in classic cmix.

Recognizes any sound file type understood by the sndlib sound file
library, including AIFF, AIFC, WAV, NeXT, Sun, IRCAM, among others. Raw
(headerless) sound files are not recognized and will produce an error
message.

-----

### See Also

[sfcreate](sfcreate.html), [sfhedit](sfhedit.html),
[sffixsize](sffixsize.html)

-----

### Authors

John Gibson \<johgibso at indiana edu\>, based on the original Cmix
**sfprint**, but revised to work with multiple soundfile header types.
Thanks to Bill Schottstaedt, whose sndlib sound file library makes this
possible.
