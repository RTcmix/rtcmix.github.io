---
title: add()
layout: ref
---

## add

Add a constant value to a table associated with a table-handle,
or do a point-by-point addition with another table-handle table.

-----

### Synopsis

**add**(*table\_handle1, table\_handle2/constant*)

-----

### Description

**add** operates on each element of the function table referred to by
*table\_handle1* by either adding the value *constant* to each one or by
summing each one with the corresponding element of the table associated
with *table\_handle2*. If the two tables are of different sizes, the
values will be interpolated relative to the length of each table prior
to the addition. The interpolation scheme used depends upon the original
setting of the optional specifier for interpolation used in the original
[maketable](maketable.html#item_optional_specifiers) scorefile command
that was used to create the table.

NOTE: The functionality of **add** has largely been duplicated by the
[pfield-enabled](../instruments/pfield-enabled.html) capabilities of
RTcmix instruments. However, the **add** function is still useful for
Perl and shell-script front-ends to RTcmix.

-----

### Arguments

  - *table\_handle1*  
      
    The table-handle identifier for the first table to be summed

  - *table\_handle2*  
      
    The table-handle identifier for the second table to be summed with
    the first, or

  - *constant*  
      
    the constant value that will be added to all the values of the first
    table.

-----

### RETURN VALUE

Returns a table-handle referring to the modified table.

-----

### Examples

``` 
   table1 = maketable("literal", "nonorm", 5, 1.0, 2.0, 3.0, 4.0, 5.0)
   table2 = maketable("literal", "nonorm", 5, 2.0, 4.0, 5.0, 8.0, 10.0)
   newtable1 = add(table1, 2.0)
   newtable2 = add(table1, table2)
```

The table-handle *newtable1* will be associated with a new table that
will contain the following sequence of elements: 3.0, 4.0, 5.0, 6.0, 7.0.
The elements of *newtable2* will be: 3.0, 6.0, 8.0, 12.0, 15.0.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[tablelen](tablelen.html), [copytable](copytable.html), [sub](sub.html),
[div](div.html), [mul](mul.html)
