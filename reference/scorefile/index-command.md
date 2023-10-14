---
title: index()
layout: ref
---

## index

Return the index of an item in an array.

-----

### Synopsis

indexnumber = **index**(*somearray, someitem*)

-----

### Description

**index** will return the index of the item *someitem* from the array
*somearray*. -1 is returned if *someitem* isn't in the *somearray*.
*someitem* can be a float, string or pfield-handle data type, and
*somearray* can be a mixture of any of these data types. **index**
counts array items starting at 0.

-----

### Arguments

  - *somearray*  
      
    the array to be checked for *someitem*

  - *someitem*  
      
    The item to find the index for in *somearray* the first, or

-----

### RETURN VALUE

Returns -1 if *sometime* is not found in *somearray*, otherwise the
index (starting at 0) of *sometime* will be returned/

-----

### Examples

After parsing he following scorefile segment:

``` 
   list = { 1, 2, "three", 4 }
   val = index(list, 2)
```

*val* will be set to 1.

-----

### See Also

[Minc](Minc.html), [len](len.html), [stringify](stringify.html),
[type](type.html)
