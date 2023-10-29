---
title: tablemax()
layout: ref
---

## tablemax

Return the maximum value stored in a table from an associated
table-handle variable.

-----

### Synopsis

maxval = **tablemax**(*table\_handle*)

-----

### Description

**tablemax** returns the maximum float value found in the table from the
*table\_handle* variable. *table\_handle* is instantiated using the
[maketable](maketable.html) scorefile command.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table.

-----

### Examples

```cpp
table = maketable("literal", "nonorm", 0, 8.00, 8.02, 8.03, 8.05, 8.07)
maxvalue = tablemax(table) \\ returns 8.07
```
-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [dumptable](dumptable.html),
[tablemin](tablemax.html), [tablelen](tablelen.html)