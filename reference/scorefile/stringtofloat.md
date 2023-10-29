---
title: stringtofloat()
layout: ref
---

## stringtofloat

Convert the numerical portion of a string into a float.

-----

### Synopsis

a_float = **stringtofloat**(*string\_to\_convert*)

-----

### Description

**stringtofloat** returns a float containing the numerical portion of a passed-in string, if found.  Otherwise it returns 0.0.

-----

### Arguments

  - *string\_to\_convert*  
    The string to convert to float.  The conversion stops at the first non-numeric character.

-----

### Examples

```cpp
a_float = stringtofloat("xxxyyy23.55zzq")  // will return 23.55
a_float = stringtofloat("abcd7DEFGH9")     // will return 7 (stops at first non-numeric)
a_float = stringtofloat("hello world")     // will return 0.0
```

-----

### See Also
[stringcontains](stringcontains.html)
