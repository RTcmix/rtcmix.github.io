---
title: copytable()
layout: ref
---

## copytable

Copy a table associated with a table-handle and
optionally resize it with or without interpolation.

-----

### Synopsis

table = **copytable**(*table\_handle*\[, *newsize*\[,
*interp\_type*\]\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**copytable** makes a copy of a table associated with *table\_handle*.
Using the optional *newsize* argument, the copied table may be larger or
smaller than the original table. The second optional argument,
*interp\_type* determines if the resized table uses interpolation or
simply truncates the values to determine the values in the new table.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table to be copied and possibly
    resized

  - *newsize*  
      
    An optional value setting the size of the copied table. If *newsize*
    is used, then the *interp\_type* should also be specified.

  - *interp\_type*  
      
    If *newsize* is specified, then the *interp\_type* can also be set.
    This is a string, identical to two of the interp options for
    [maketable](maketable.html):
      - *"interp"/"nointerp"* &mdash; *"interp"* enables simple first-order
        linear interpolation, i.e. if a requested table value lies
        between two elements in the table, setting *"interp"* will
        generate an intermediate value based upon a linear interpolation
        of nearest sample values. For example, if the value at table
        location 314.15 were requested, the value returned would be 0.15
        between the value at location 314 and the value at location 315.
        *"nointerp"* turns off interpolation; the values returned from
        the table will be 'rounded-down' (truncated) to the
        nearest-lowest point in the table. For example, if the table
        value at location 149.78 were requested, the *"nointerp"* option
        would return the value stored at location 149.
    The default is *interp*.

-----

### RETURN VALUE

Returns a table-handle for the new, copied and (possibly) resized table

-----

### Examples

```cpp
   table = maketable("wave", 4000, 1.0, 0.2, 0.1)
   newtable = copytable(table, 1000, "interp")
```

*newtable* will be associated with a new table that will contain the
same waveform defined in the first **maketable**, except that it will
only fill 1000 elements instead of 4000. The new values will be
determined by linear interpolation if they don't align with the values
taken from the original 4000-point table.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [makeconverter](makeconverter.html),
[plottable](plottable.html), [dumptable](dumptable.html),
[tablelen](tablelen.html), [mul](mul.html), [div](div.html),
[sub](sub.html), [add](add.html)
