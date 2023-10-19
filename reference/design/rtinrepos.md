---
title: rtinrepos()
layout: ref
---

### rtinrepos

*INSTRUMENT design -- reposition input pointer for arbitrary
file-reading*  
  
The **rtinrepos()** function is used in RTcmix instrument design to set
the input point for reading. This is done in preparation for a
subsequent call to [rtgetin](rtgetin.html). Designed for use by
instruments that want to reposition the input file arbitrarily (like
[REVMIX](../instruments/REVMIX.html), which reads a file backwards).

Typically it is used in the *INSTRUMENT::run()* member function of an
instrument. It only works on file input, as arbitrary movement forwards
and backwards in real-time isn't quite possible (yet).

It replaces the older [inrepos](inrepos.html) function used in disk-only
cmix.

-----

### Usage

**int rtinrepos(***Instrument \*inst, int nframes, int whence***)**  
  

<u>*\*inst*</u> is a pointer to the INSTRUMENT object being scheduled.
Usually this is the token *this* (i.e. a pointer to the INSTRUMENT
calling the **rtinrepos()** function).  
<u>*nframes*</u> is the number of samples to move the input pointer
forwards or backwards from a point set by *whence*. *nframes* can take a
negative value.  
<u>*whence*</u> works the same way that the unix *lseek()* function
does:

  - If *whence* is **SEEK\_SET**, then the input read pointer will be
    set *nframes* from the beginning of the soundfile.
  - If *whence* is **SEEK\_CUR**, the the input read pointer will be set
    *nframes* from the current read position (positive or negative
    values).  
      
    *\[note: The values of **SEEK\_SET** and **SEEK\_CUR** are defined
    in the system header file "unistd.h". **SEEK\_END** is not defined
    for use by* rtinrepos()*.\]*  

  
  
The **rtinrepos()** function returns *0* if it is successful, it will
exit if there is an error.

-----

### Examples

  

-----

### See Also

  - [rtgetin](rtgetin.html)
