---
title: wrap()
layout: ref
---

## wrap

Return the remainder of integer division.

-----

### Synopsis

val = **wrap**(*value, range*)

-----

### Description

**wrap** returns the modulus of *value*, using *range* as the modulo;
i.e., it will keep *value* within *range* by "wrapping" it around.

See also [mod](mod.html) which apparently does the same thing.

-----

### Arguments

  - *value, range*  
    The parameters used to perform the "wrapping" (i.e., *a mod b;* in
    this case, *value mod range*) operation. Essentially **wrap** gives
    the remainder of dividing *value* into *range*. *Modular* (or
    "clock") arithmetic is very useful in western, 12-note/octave music.

-----

### Examples

```cpp
   wrappedval = wrap(7, 12)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [max](max.html),
[mod](mod.html), [min](min.html), [round](round.html),
[trunc](trunc.html)
