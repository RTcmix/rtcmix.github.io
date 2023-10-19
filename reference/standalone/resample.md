---
title: resample()
layout: ref
---

**resample** - convert sampling rate

-----

### Usage

**resample** **-r** *NEW\_SRATE \[options\]* *inputfile \[outputfile\]*

-----

### Description

**resample** can do two kinds of sampling rate conversion: (1)
Kaiser-windowed low-pass filter (better) (2) linear interpolation only,
no filter (faster)

For (1), use either no option, in which case you get the default,
decent-quality resampling filter; use the *-a* option for a better
quality filter; or use a combination of the *-f*, *-b* and *-l* options
to design your own filter. For (2), use the *-i* option.

-----

### Options

Here are the options:

``` 
  Options:                                                           
     -a       triple-A quality resampling filter                     
  OR:                                                                
     -f NUM   rolloff Frequency (0 < freq <= 1)       [default: 0.9] 
     -b NUM   Beta ( >= 1)                            [default: 9.0] 
     -l NUM   filter Length (odd number <= 65)        [default: 65]  
                                                                     
     -n       No interpolation of filter coefficients (faster)       
     -i       resample by linear Interpolation, not with filter      
     -t       Terse (don't print out so much)                        
     -v       print Version of program and quit                      
  If no output file specified, writes to "inputfile.resamp".
```
