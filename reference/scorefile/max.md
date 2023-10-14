---
title: max()
layout: ref
---

## max

Return maximum value of params.

-----

### Synopsis

maxval = **max**(*p0, p1, p2, ...*)

-----

### Description

**max** returns the maximum of all the parameters *p0, p1, ... etc.*
passed to it.

-----

### Arguments

  - *p0, p1, ..., pN*  
    The parameters passed into **max** may be any number, floating point
    or integer. **max** will not work on arrays or tables.

-----

### Examples

``` 
   mval = max(7, 0, 8.9, -14, 78.7878)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [min](min.html),
[mod](mod.html), [round](round.html), [trunc](trunc.html),
[wrap](wrap.html)
