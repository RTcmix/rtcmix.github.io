---
title: samptable
layout: ref
---

## samptable

Return a specified value from a table given an
associated table-handle variable.

-----

### Synopsis

val = **samptable**(*table\_handle*, \[ *interp\_type,* \] *index*)

Parameters inside the \[brackets\] are optional.

-----

### Description

**samptable** returns the value at table element *index* of the table
from the *table\_handle* variable. *table\_handle* is instantiated using
the [maketable](maketable.html) scorefile command. The optional
*interp\_type* specifier determines if a fractional *index* will be
interpolated between two of the table values, or will be 'rounded down'
(truncated) to the nearest integer table location.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table.

  - *interp\_type*  
      
    This optional string argument can be either *"interp"* or
    *"nointerp"*, with the *"interp"* specifying a simple linear
    interpolation between adjacent table values if the requested
    table-location *index* is fractional. *"nointerp"* will simply round
    downwards (truncate the fractional part) to return a value.
    
    *"interp"* is the default setting.

  - *index*  
      
    If *index* is an integer or of the optional *"nointerp"* specifier
    has been set, then **sampfunc** will return the value of the table
    at location *index* in the table array. An *index* with a fractional
    part will return a linear-interpolated value between two adjacent
    (integer)*index* locations in the table array. Note that these table
    arrays begin numbering at 0.

-----

### RETURN VALUE

Returns to the script the value at the *index* (or interpolated between
*index* and *index+1*) in the table array.

-----

### Examples

``` 
   table = maketable("literal", "nonorm", 0, 8.00, 8.02, 8.03, 8.05, 8.07)
   tablelength = tablelen(table)

   for (i = 0; i < 10; i = i+1) {
      val = samptable(table, "nointerp", irand(0, tablelength))
   }
```

*val* will be set to a different, random value of the *table* for each
iteration of the loop. Setting the optional specifier *"nointerp"* will
guarantee that *val* will assume values taken directly from the *table*.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makefilter](makefilter.html), [copytable](copytable.html),
[dumptable](dumptable.html), [tablelen](tablelen.html)
