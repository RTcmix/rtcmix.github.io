---
title: fwrap()
layout: ref
---

## fwrap

Return the wrapped float value within a float range.

-----

### Synopsis

val = **fwrap**(*value, range*)

-----

### Description

**fwrap** returns the [float](Minc.html#float) result of wrapping the value to fit between 0.0 and *range*;
i.e., it will keep *value* within *range* by "wrapping" it around.

Note that this is *not* the same action as [wrap](wrap.html), which effectively performs a [modulo](Minc.html) operation.

-----

### Arguments

  - *value, range*  
    The parameters used to perform the "wrapping".  If *value* exceeds *range*, *range* is repeatedly subtracted from *value* until that condition is no longer true.  If *value* is less than 0.0, *range* is repeatedly added in the same fashion.

-----

### Examples

```cpp
wrappedval = wrap(127.234, 11.787)	// returns 9.36399364471
```

-----

### See Also

[abs](abs.html), [pow](pow.html), [max](max.html), [min](min.html), [wrap](wrap.html)
