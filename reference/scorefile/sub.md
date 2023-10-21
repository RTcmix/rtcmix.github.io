---
title: sub()
layout: ref
---

## sub

Subtract a constant value from a table associated with a
table-handle or do a point-by-point subtraction from another
table-handle table.

-----

### Synopsis

**sub**(*table\_handle1*, *table\_handle2/constant*)

-----

### Description

**sub** operates on each element of the function table referred to by
*table\_handle1* by either subtracting the value *constant* from each
one or by subtracting the corresponding element of the table associated
with *table\_handle2* from each one. If the two tables are of different
sizes, the values will be interpolated relative to the length of each
table prior to the subtraction. The interpolation scheme used depends
upon the original setting of the optional specifier for interpolation
used in the original
[maketable](maketable.html#item_optional_specifiers) scorefile command
that was used to create the table.

-----

### Arguments

  - <span id="item_table_handle1">*table\_handle1*</span>  
      
    The table-handle identifier for the first table (the one subtracted
    from).

  - <span id="item_table_handle2">*table\_handle2*</span>  
      
    The table-handle identifier for the second table to be subtracted
    from the first, or

  - <span id="item_constant">*constant*</span>  
      
    the constant value that will be subtracted from all the values of
    the first table

-----

### RETURN VALUE

Returns a table-handle referring to the modified table

-----

### Examples

```cpp
   table1 = maketable("literal", "nonorm", 5, 1.0, 2.0, 3.0, 4.0, 5.0)
   table2 = maketable("literal", "nonorm", 5, 2.0, 4.0, 5.0, 8.0, 10.0)
   newtable1 = sub(table1, 2.0)
   newtable2 = sub(table1, table2)
```

The table-handle *newtable1* will be associated with a new table that
will contain the following sequence of elements: -1.0, 0.0, 1.0, 2.0, 3.0.

and the elements of *newtable2* will be: -1.0, -2.0, -2.0, -4.0, -5.0.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[tablelen](tablelen.html), [copytable](copytable.html), [add](add.html),
[div](div.html), [mul](mul.html)
