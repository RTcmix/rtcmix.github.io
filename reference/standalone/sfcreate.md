---
title: sfcreate()
layout: ref
---

**sfcreate** - create soundfile header, or change existing one

-----

### Synopsis

**sfcreate** \[ **-r** *sampling\_rate* \] \[ **-c** *num\_channels* \]
\[ **-t** *header\_format* \] \[ **-i** | **-f** \] \[ **-b** | **-l**
\] \[ **--force** \] *file\_name*

-----

### Description

If *file\_name* doesn't exist, **sfcreate** creates a new file with the
specified soundfile header. If *file\_name* already exists, **sfcreate**
puts a soundfile header at the beginning of the file, overwriting
whatever was there. (See **WARNINGS** below.) Without a *file\_name*
argument, **sfcreate** just prints a help summary.

This command is mainly useful for disk-based Cmix scripts, in which the
**output** function expects to find a file with a soundfile header. You
can run **sfcreate** from within such a script by using the
[system](../scorefile/system.html) function.

-----

### Options

  - **-b**  
      
    Use big-endian sample data format. This is the default for all file
    types except *wav*. (Use either this or **-l**.)

  - **-c** *num\_channels*  
      
    Use *num\_channels* number of channels. The default is 2.

  - **-f**  
      
    Use floating-point sample data format. (Use either this or **-i**.)

  - **-i**  
      
    Use 16-bit (short) integer sample data format. This is the default.
    (Use either this or **-f**.)

  - **-l**  
      
    Use little-endian sample data format. This is the default for **-t**
    *wav*. (Use either this or **-b**.)

  - **-r** *sampling\_rate*  
      
    Use the sampling rate given by *sampling\_rate*. 44100 is the
    default.

  - **-t** *header\_format*  
      
    Use the specified file format. The possibilities are:
    
      - *aiff*  
          
        AIFF format
      - *aifc*  
          
        AIFC format
      - *wav*  
          
        Microsoft RIFF (Wav) format
      - *next*  
          
        NeXT format (same as *sun*)
      - *sun*  
          
        Sun "au'' format (same as *next*)
      - *ircam*  
          
        IRCAM format
    
    *aiff* (or *aifc* for floats) is the default.

  - **--force**  
      
    Overwrite the header of an existing file, even if this might result
    in swapped channels or corrupted sample words. You will be told if
    these things have happened after they've happened. (Isn't that
    nice\!)

-----

### Examples

-----

### See Also

[sfprint](sfprint.html), [sfhedit](sfhedit.html),
[sffixsize](sffixsize.html)

-----

### Authors

John Gibson \<johgibso at indiana edu\>, based on the original Cmix
**sfcreate**, but revised to work with multiple soundfile header types.
Thanks to Bill Schottstaedt, whose sndlib sound file library makes this
possible.
