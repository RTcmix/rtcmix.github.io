---
title: f\_arg()
layout: ref
---

## f\_arg / i\_arg / s\_arg / n\_arg

Legacy functions for returning command-line arguments.

-----

### Synopsis

val = **f\_arg**(*arg\_index*)  
val = **i\_arg**(*arg\_index*)  
val = **s\_arg**(*arg\_index*)  
val = **n\_arg**()

-----

### Description

These functions allow command-line arguments to the *CMIX* command to be
entered into the RTcmix scorefile environment. For example, the command:

contains 5 ingeter command-line arguments. The command:

contains 2 floating-point arguments and 1 string argument.

**f\_arg**(*n*) returns the *n*th argument as a floating-point value.  
**i\_arg**(*n*) returns the *n*th argument as an integer value.  
**s\_arg**(*n*) returns the *n*th argument as a string value.  
**n\_arg**() returns the number of arguments in the *CMIX* command.

-----

### Arguments

  - *arg\_index*  
    The index of the argument to retrieve. Numbering of arguments starts
    at "0" for the first argument after the *CMIX* command.

-----

### Examples

```cpp
   floatval = f_arg(3)
   stringval = s_arg(1)
   nargs = n_arg()
```
-----

*Note:  These functions have been replaced in [Minc](Minc.html) by the 
much-more-flexible command-line named arguments described* <a href="Minc.html#command-line-named-args" >here</a>.

-----

### See Also

[print](print.html), [printf](printf.html),
[rtsetparams](rtsetparams.html), [set\_option](set_option.html),
[stringify](stringify.html), [type](type.html), [Minc](Minc.html)
