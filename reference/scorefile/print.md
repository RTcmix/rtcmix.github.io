---
title: print()
layout: ref
---

## print

Print the values of the arguments.

-----

### Synopsis

**print**(*somevar*\[, *othervars...*\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**print** will print the interpreted values of its arguments to the
screen. The command will interpret various [Minc](Minc.html) data-types
and attempt to produce an accurate representation of their values. It
will print multiple values on a single output line if multiple values
are given as input. **print** returns 0 to the Minc environment.

**print** will ignore the value set for printing using the
[set\_option](set_option.html#print), [print\_on](print_on.html) or
[print\_off](print_off.html) commands and always print its arguments.

-----

### Arguments

  - <span id="item_vars">*somevar, othervars*</span>  
      
    The variable (or number, etc.) whose value will be printed

-----

### Examples

The following scorefile:

```
   a = 7.8
   b = 8.9
   c = {1, 2, 3}
   print(a, b, c)
```

will produce the following output:

```
   7.8, 8.9, [1, 2, 3]
```

-----

### See Also

[printf](printf.html), [type](type.html), [dumptable](dumptable.html),
[set\_option](set_option.html#print), [print\_on](print_on.html),
[print\_off](print_off.html)
