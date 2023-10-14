---
title: tablelen()
layout: ref
---

## tablelen

Return the size of a table from an associated
table-handle variable.

-----

### Synopsis

val = **tablelen**(*table\_handle*)

-----

### Description

**tablelen** returns the size (length) of the table from the
*table\_handle* variable. *table\_handle* is instantiated using the
[maketable](maketable.html) scorefile command.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table.

-----

### RETURN VALUE

Returns to the script an integer value, the size of the table.

-----

### Examples

``` 
   table = maketable("literal", "nonorm", 0, 8.00, 8.02, 8.03, 8.05, 8.07)
   size = tablelen(table)
```

*size* will have the value 5, corresponding to the 5 elements of the
table. This is very useful when a *size* argument of 0 is used for a
[maketable("literal", ...)](maketable.html#literal) table type.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [dumptable](dumptable.html),
[tablelen](tablelen.html), [mul](mul.html), [div](div.html),
[sub](sub.html), [add](add.html), [len](len.html)
