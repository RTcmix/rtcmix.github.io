---
title: tablemin()
layout: ref
---

## tablemin

Return the minimum value stored in a table from an associated
table-handle variable.

-----

### Synopsis

minval = **tablemin**(*table\_handle*)

-----

### Description

**tablemin** returns the minimum float value found in the table from the
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
maxvalue = tablemin(table) \\ returns 8.00
```
-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [dumptable](dumptable.html),
[tablemax](tablemax.html), [tablelen](tablelen.html)