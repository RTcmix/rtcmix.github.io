---
title: sfhedit()
layout: ref
---

**sfhedit** - edit soundfile header

-----

### Synopsis

**sfhedit** \[ **-c** *num\_channels* \] \[ **-i** | **-f** \] \[ **-p**
\] \[ **-r** *sampling\_rate* \] \[ **-w** \] \[ **-z** \] *file\_name*

-----

### Description

**sfhedit** lets you edit some parts of a soundfile header. Without a
*file\_name* argument, **sfhedit** just prints a help summary.

-----

### Options

  - ****-c** *num\_channels***  
      
    Set the number of channels to *num\_channels*.
  - ****-f****  
      
    Set the sample data format to floating-point.
  - ****-i****  
      
    Set the sample data format to 16-bit (short) integer.
  - ****-p****  
      
    Interactively asks you for the peak amplitude and frame location for
    each channel.
  - ****-r** *sampling\_rate***  
      
    Set the sampling rate to *sampling\_rate*.
  - ****-w****  
      
    Throws you into vi so you can edit the comment.
  - ****-z****  
      
    Truncates the data in the file, leaving only the header.

-----

### Bugs

There are currently some problems with this program. Don't use it on
anything irreplaceable.

-----

### See Also

[sfprint](sfprint.html), [sfcreate](sfcreate.html),
[sffixsize](sffixsize.html)
