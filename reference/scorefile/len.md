---
title: len()
layout: ref
---

## len

Return the length of the argument.

-----

### Synopsis

val = **len**(*item*)

-----

### Description

**len** will return the length of the argument *item*. If it is a Minc
list, it returns the number of elements in the list. If it is a string, it
returns the number of characters in the string. For other types it returns
"1". (Note: this includes pfield-handles and table-handles &mdash; see the
[tablelen](tablelen.html) scorefile command for accessing table lengths).

-----

### Arguments

  - *item*  
    The item to be checked for length

-----

### Examples

After parsing he following scorefile segment:

```cpp
   arr = { 1, 2, 3 }
   val1 = len(arr)
   val2 = len("hey there")
```

*val1* will be set to 3, and *val2* will be set to 9.

-----

### See Also

[tablelen](tablelen.html), [type](type.html)
