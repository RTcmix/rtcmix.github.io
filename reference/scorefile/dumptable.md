---
title: dumptable()
layout: ref
---

## dumptable

Print the contents of a table from an associated
table-handle variable to a terminal or a file.

-----

### Synopsis

table = **dumptable**(*table\_handle*\[, *"filename"*\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**dumptable** is a useful utility that will print the contents of a
table associated with a particular table-handle to the screen, or to a
text file if the optional *filename* is given.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table.

  - *filename*  
      
    This optional string argument refers to a file that will contain the
    printed output from the **dumptable** command. This text file can be
    identified using an absolute or a relative path &mdash; i.e., if the
    *filename* is given with no path information, the file will be
    created in the directory where the scorefile is invoked.

-----

### RETURN VALUE

Returns 0.0 if the printing is successful, -1.0 if there was an error.

-----

### Examples

```cpp
   table = maketable("wave", 10, 1)
   dumptable(table)
```

The above score will produce the following output to the screen:

```cpp
   0 0.000000
   1 0.618034
   2 1.000000
   3 1.000000
   4 0.618034
   5 0.000000
   6 -0.618034
   7 -1.000000
   8 -1.000000
   9 -0.618034
```

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [tablelen](tablelen.html), [mul](mul.html),
[div](div.html), [sub](sub.html), [add](add.html)
