---
title: mod()
layout: ref
---

## mod

Return the remainder of integer division.

-----

### Synopsis

modval = **mod**(*p0, p1*)

-----

### Description

**mod** returns the result of the operation *p0 mod p1* (otherwise known
as: *p0 % p1*). **mod** converts both params to integers.

The [wrap](wrap.html) command apparently does the same thing.

-----

### Arguments

  - *p0, p1*  
    The parameters used to perform the *a mod b* (in this case, *p0 mod
    p1*) operation. Essentially **mod** gives the remainder of dividing
    *p0* into *p1*. *Modular* (or "clock") arithmetic is very useful in
    western, 12-note/octave music.

-----

### Examples

``` 
   modout = mod(3, 12)
```

-----

### See Also

[abs](abs.html), [log](log.html), [pow](pow.html), [max](max.html),
[min](min.html), [round](round.html), [trunc](trunc.html),
[wrap](wrap.html)
