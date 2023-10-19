---
title: rescale()
layout: ref
---

**rescale** - convert 32-bit float to 16-bit integer sound file

-----

### Synopsis

**rescale** \[ **-P** *desired\_peak* \] \[ **-p** *input\_peak* \] \[
**-f** *factor* \] \[ **-r** \] \[ **-t** \] \[ **-s** *input\_skip* \]
\[ **-o** *output\_skip* \] \[ **-d** *duration* \] \[ **-e**
*end\_silence* \] \[ **-h** \] *input\_file* \[ *output\_file* \]

-----

### Description

This command-line utility converts a 32-bit floating-point sound file to
16-bit integer. It does this by first scaling every sample by a factor,
and then chopping off the portion to the right of the decimal point. (So
if the factor is 1, a sample of 22091.428915 becomes 22091.)

There are several options that affect what factor **rescale** uses,
where the output goes, and whether to dither before truncating from 32
bits.

-----

### Options

  - **-P** *desired\_peak*  
      
    Rescale so that *desired\_peak* is the new peak. Ignored if you also
    specify *factor*. This is 32767 by default.

  - **-p** *input\_peak*  
      
    Specify the peak of the input file. This is taken from the input
    file header by default.

  - **-f** *factor*  
      
    Multiply every sample value by this factor before converting to
    integer. This is *desired\_peak* / *input\_peak* by default.

  - **-r**  
      
    Replace *input\_file* with the rescaled version. Ignores
    *output\_file*. By default, **rescale** writes to a new file. (See
    *output\_file* below.)

  - **-t**  
      
    Use dithering algorithm. This is off by default.

  - **-s** *input\_skip*  
      
    Skip this many seconds on the input file before reading. This is 0
    by default.

  - **-o** *output\_skip*  
      
    Skip this many seconds on the output file before writing. This is 0
    by default.

  - **-d** *duration*  
      
    Rescale this many seconds of the input file. This is the entire file
    by default.

  - **-e** *end\_silence*  
      
    Write this many seconds of zeros at the end of the output file. This
    is 0 by default.

  - **-h**  
      
    Print usage summary.

  - **input\_file**  
      
    The name of the input file, which can be either 32-bit floating
    point or 16-bit integer in any of the formats understood by RTcmix.

  - **output\_file**  
      
    Write rescaled output to this file, which cannot already exist.
    Ignored if **-r** also specified. If neither **-r** nor
    **output\_file** given, rescale writes to a new file with the same
    name as *input\_file*, but with ".rescale'' appended.
    
    This file has the same header format as the input file, as long as
    that is one of the types that RTcmix can write (AIFF, AIFC, WAVE,
    NeXT, IRCAM). If it's a type that RTcmix can read, but not write,
    then the output format will be AIFF.

-----

### Examples

``` 
   rescale foo.wav
```

Assuming "foo.wav'' is a 32-bit float file, this command rescales the
file so that its peak amplitude is 32767, and writes the 16-bit output
to "foo.wav.rescale.''

``` 
   rescale -r -f 1 foo.wav
```

rescales "foo.wav'' using a factor of 1.0 (i.e., it merely drops digits
of precision to the right of the decimal point), and writes the output
to "foo.wav,'' overwriting the original 32-bit file.

``` 
   rescale -P 20000 -t foo.wav newfoo.wav
```

rescales "foo.wav'' so that its peak amplitude is 20000, and writes the
output to a new file, "newfoo.wav.'' Before truncating to 16 bits,
applies the dithering algorithm to each sample.

-----

### Authors

John Gibson \<johgibso at indiana edu\>, based on the original Cmix
**rescale**, but revised and expanded.
