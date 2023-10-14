---
title: trunc()
layout: ref
---

## trunc

Truncate floating-point value of argument to integer.

-----

### Synopsis

val = **trunc**(*someval*)

-----

### Description

**trunc** returns the truncated (integer part) value of *someval*. Like
all Minc functions, however, it returns it as a double (floating-point)
value. For example

``` 
val = trunc(14.154978)
```

will set *val* to 14.0. This differs from the [round](round.html)
scorefile command in that [round](round.html) will return the closest
int, where [trunc](trunc.html) simply ignores the values after the
decimal point.

-----

### Arguments

  - *someval*  
    Any number, floating point or integer (although an int would be
    silly...)

-----

### Examples

``` 
   truncval = trunc(arbitrary_val)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [max](max.html),
[min](min.html), [mod](mod.html), [round](round.html), [wrap](wrap.html)
