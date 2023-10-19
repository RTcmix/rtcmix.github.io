---
title: div()
layout: ref
---

## div

Divide a table associated with a table-handle by a constant, or
do a point-by-point division by another table-handle table.

-----

### Synopsis

**div**(*table\_handle1*, *table\_handle2/constant*)

-----

### Description

**div** operates on each element of the function table referred to by
*table\_handle1* by either dividing by the value *constant* to each one
or by dividing each one by the corresponding element of the table
associated with *table\_handle2*. If the two tables are of different
sizes, the values will be interpolated relative to the length of each
table prior to the division. The interpolation scheme used depends upon
the original setting of the optional specifier for interpolation used in
the original [maketable](maketable.html#item_optional_specifiers)
scorefile command that was used to create the table.

NOTE: The functionality of **div** has largely been duplicated by the
[pfield-enabled](../instruments/pfield-enabled.html) capabilities of
RTcmix instruments. However, the **div** function is still useful for
Perl and shell-script front-ends to RTcmix.

-----

### Arguments

  - *table\_handle1*  
      
    The table-handle identifier for the first table (the
    [dividends](http://www.math.com/school/glossary/defs/dividend.html)
    to be used).

  - *table\_handle2*  
      
    The table-handle identifier for the second table to be divided into
    the first (the
    [divisors](http://www.math.com/school/glossary/defs/divisor.html)),
    or

  - *constant*  
      
    the constant value that will be divided into to all the values of
    the first table.

-----

### RETURN VALUE

Returns a table-handle referring to the modified table. If the second
value is ever 0, then the value 999999999999999999.9 is returned. Hey,
it's *almost* infinity... right?

-----

### Examples

``` 
   table1 = maketable("literal", "nonorm", 5, 1.0, 2.0, 3.0, 4.0, 5.0)
   table2 = maketable("literal", "nonorm", 5, 2.0, 4.0, 5.0, 8.0, 10.0)
   newtable1 = div(table1, 2.0)
   newtable2 = div(table1, table2)
```

The table-handle *newtable1* will be associated with a new table that
will contain the following sequence of elements: 0.5, 1.0, 1.5, 2.0, 2.5.
The elements of *newtable2* will be: 0.5, 0.5, 0.6, 0.5, 0.5.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[tablelen](tablelen.html), [copytable](copytable.html), [add](add.html),
[sub](sub.html), [mul](mul.html)
