---
title: min()
layout: ref
---

## min

Return minimum value of params.

-----

### Synopsis

minval = **min**(*p0, p1, p2, ...*)

-----

### Description

**min** returns the minimum of all the parameters *p0, p1, ... etc.*
passed to it.

-----

### Arguments

  - *p0, p1, ..., pN*  
    The parameters passed into **min** may be any number, floating point
    or integer. **min** will not work on arrays or tables.

-----

### Examples

```cpp
   mval = min(7, 0, 8.9, -14, 78.7878)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [max](max.html),
[mod](mod.html), [round](round.html), [trunc](trunc.html),
[wrap](wrap.html)
