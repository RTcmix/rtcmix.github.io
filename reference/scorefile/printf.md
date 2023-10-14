---
title: printf()
layout: ref
---

## printf

Print using a C-like format.

-----

### Synopsis

**printf**(*"format string"*, *var1*\[, *var2 ...*\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**printf** will print a formatted version of the values its arguments to
the screen using a syntax similar to the C "printf" command. The
**printf** formatting can truncate floating point numbers to integers in
the output. **printf** returns 0 to the Minc environment.

**printf** will ignore the value set for printing using the
[set\_option](set_option.html#print), [print\_on](print_on.html) or
[print\_off](print_off.html) commands and always print its arguments.

-----

### Arguments

  - <span id="item_format_string">*"format string"*</span>  
      
    The C-syntax format string (i.e., "%d, %f, %3.2f, %s, \\n" .. etc.)
    used to format the output that will appear on the screen.

  - <span id="item_vars">*var1, var2...*</span>  
      
    The variables to be printed, corresponding to the format string.

-----

### Examples

The following scorefile:

```
    a = 7.8
    printf("the value of a is: %f, %s\n", a, "so THERE!")
```

will produce the following output:

```
    the value of a is: 7.8, so THERE!
```

-----

### See Also

[print](print.html), [type](type.html), [dumptable](dumptable.html),
[set\_option](set_option.html#print), [print\_on](print_on.html),
[print\_off](print_off.html)
