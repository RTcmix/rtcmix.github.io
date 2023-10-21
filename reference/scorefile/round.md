---
title: round()
layout: ref
---

## round

Round the argument to the nearest integer.

-----

### Synopsis

val = **round**(someval)

-----

### Description

**round** returns (as a double) the nearest integer ("rounded off")
value of the argument *someval*. That is, 3.789 becomes 4.0; 987.123 becomes
987.0. This differs from the [trunc](trunc.html) scorefile command in
that it will return the closest int, where [trunc](trunc.html) simply
ignores the values after the decimal point.

-----

### Arguments

  - *someval*  
    Any number, floating point or integer (although an int would be
    silly...)

-----

### Examples

```cpp
   roundedval = round(arbitrary_val)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [max](max.html),
[min](min.html), [mod](mod.html), [trunc](trunc.html), [wrap](wrap.html)
