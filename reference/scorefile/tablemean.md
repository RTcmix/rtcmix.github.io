---
title: tablemean()
layout: ref
---

## tablemean

Return the mean (average) of all values stored in a table from an associated
table-handle variable.

-----

### Synopsis

table\_average = **tablemean**(*table\_handle*)

-----

### Description

**tablemean** returns the average of all float values found in the table from the
*table\_handle* variable. *table\_handle* is instantiated using the
[maketable](maketable.html) scorefile command.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table.

-----

### Examples

```cpp
table = maketable("literal", "nonorm", 0, 10, 20, 30, 40)
table_average = tablemean(table) \\ returns 25
```
-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [dumptable](dumptable.html),
[tablemin](tablemin.html), [tablemax](tablemax.html), [tablelen](tablelen.html)