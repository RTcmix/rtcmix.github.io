---
title: modtable()
layout: ref
---

## modtable

Modify the data coming through one *table-handle* and
send the transformed data through another *table-handle*.

-----

### Synopsis

table = **modtable**(*input\_table\_handle*, *"modification\_type"*,
*arg1, arg2, ...*)

-----

### Description

**modtable** returns a *table-handle* that will deliver data produced by
modifying the values coming from another table. The specific
data-transformation applied is determined by the *"modification\_type"*
text field in conjunction with any of the optional *argN* parameters.
The *input\_table\_handle* variable should refer to another table via a
*table-handle*, probably created using the [maketable](maketable.html))
command or possibly coming from a previous **modtable**.

Many of the arguments for **modtable** may themselves also be
pfield-handles.

-----

### Arguments

  - *input\_table\_handle*  
      
    This is a *table-handle* variable referring to a data stream coming
    from a previously created or modified table.

  - *modification\_type*  
      
    This string value (i.e., enclosed in "double quotes" in the
    scorefile) determines the operation that will be used to transform
    the values coming through the *input\_table\_handle*. The
    *table-handle* returned from the **modtable** command will refer to
    this transformed table data.

  - *arg1, arg2, ...*  
      
    The arguments that are possibly relevant for each modification type.
    These will determine how particular operations work to transform the
    data. See documentation for each type below.

-----

## Modification Types

  - <span id="concat" class="internallink">*concat*</span>  
      
    The *concat* operation concatenates the two table arguments:
	 The values for *table2* are appended to those of *table_handle*
	 and stored into a new table. The syntax is:

```
    table = modtable(table_handle, "concat", table2)
```
    
    The values of the table are not altered in any other way. The
    size of the new table is the total of the two input tables. This
    returns a table-handle for the concatenated table data.

  - <span id="draw" class="internallink">*draw*</span>  
      
    The *draw* modtable variant enables you to rewrite a portion of the
    *table-handle* table "on the fly" during scorefile execution,
    possibly while an Instrument might be accessing the table. The
    syntax is:

```
    table = modtable(table_handle, "draw", ["literal",] index, value[, width])
```
    
    The elements of the table referenced by *table\_handle* will be
    replaced by this command whenever an Instrument asks for a value
    from the *table* returned, usually at the control rate. The
    replacement operation will be governed by the *index*, which is
    normally 0&ndash;1 and acts as a fractional portion into the table to
    modify (i.e., 0.5 will replace the values halfway through the table)
    unless the optional *"literal"* argument is present. Then *index*
    represents an absolute index into the table, starting at 0 and
    ending at table\_length-1. *value* is the value to place at the
    *index*, and the optional *width* parameter determines how the
    neighboring slots might be affected (linear interpolation) by the
    newly-inserted *value*. *width* is also a fractional portion from
    0&ndash;1, with the default set at 0. The interpolation algorithm used by
    *draw* will affect elements in the table that are (*width*\*size)/2
    slots away on either side of the *index*. Both the *index* and the
    *value* can also be dynamic pfield-handles.
    
    The table referenced by *table-handle* will itself be modified, so
    subsequent uses of the *table-handle* table will reflect whatever
    changes might have been "drawn" into the table during previous
    scorefile processing. Modification of the table will only occur when
    the pfield variable (*table*) is accessed in a score, however.
    
      
  - <span id="normalize" class="internallink">*normalize*</span>  
      
    The *normalize* modification type processes the data from a table
    associated with *table\_handle* and normalizes (scales) the values
    using the the optional *peak* argument as the desired peak value for
    the elements in the new table. The syntax is:

```
    table = modtable(table_handle, "normalize"[, peak])
```
    
    If the optional *peak* argument is missing, then 1.0 will be used as
    the desired peak value.
    
    Tables can have both positive and negative values. This is what
    happens, depending on the sign of the values in the table:  

    | sign of values        | resulting range of values            |
    | --------------------- | ------------------------------------ |
    | all positive          | between 0.0 and <i>peak</i>          |
    | all negative          | between 0.0 and <i>-peak</i>         |
    | positive and negative | between <i>-peak</i> and <i>peak</i> |
    
    A new table-handle will be returned for subsequent use in the
    scorefile referring to the normalized table values.

  - <span id="reverse" class="internallink">*reverse*</span>  
      
    The *reverse* operation reverses the ordering of all the values in a
    table associated with *table\_handle*, essentially the same as
    reading through the original table backwards. The syntax is:

```
    table = modtable(table_handle, "reverse")
```
    
    The values will be interpolated depending upon the original setting
    of the optional specifier for interpolation used in the
    [maketable](maketable.html#item_optional_specifiers) scorefile
    command that created the table.
    
    The values of the table are not altered in any other way. The size
    of the table is also unchanged. This returns a table-handle for the
    reversed table data.

  - <span id="shift" class="internallink">*shift*</span>  
      
    The *shift* modification type operates on the table associated with
    *table\_handle* and moves all the elements in the table forwards or
    backwards (depending on whether *shift\_amount* is positive or
    negative) in the table. The syntax is:

```
    table = modtable(table_handle, "shift", shift_amount)
```
    
    The elements in the table array will be moved *shift\_amount* array
    locations. Positive values of *shift\_amount* shift to the right;
    negative values to the left. If a value is shifted off the end of
    the array in either direction, it reenters the other end of the
    array.
    
    The values of the table are not altered in any other way. A
    table-handle referring to the shifted table values is returned.


-----

### Examples

Using these scorefile commands:

``` 
   table = maketable("line", "nonorm", 1000, 0,0, 1,10, 2, -5)
   newtable = modtable(table, "normalize", 1.0)
```

*newtable* will be associated with a new table that will start at 0.0,
go up to 1.0, and then down to -0.5 over the 1000 elements in the table.

In this set of commands:

``` 
   table = maketable("literal", "nonorm", 5, 1.0, 2.0, 3.0, 4.0, 5.0)
   newtable = modtable(table, "reverse")
```

the table-handle *newtable* will be associated with a new table that
will contain the following sequence of elements:

``` 
   5.0, 4.0, 3.0, 2.0, 1.0
``` 

And this scorefile fragment:

``` 
   table = maketable("literal", "nonorm", 5, 1.0, 2.0, 3.0, 4.0, 5.0)
   newtable1 = modtable(table, "shift", 3)
   newtable2 = modtable(table, "shift", -1)
```

will result in the table-handle *newtable1* being associated with a new
table that will contain the following sequence of elements:

``` 
   3.0, 4.0, 5.0, 1.0, 2.0
``` 

and the elements of *newtable2* being in the following order:

``` 
   2.0, 3.0, 4.0, 5.0, 1.0
``` 


-----

### See Also

[maketable](maketable.html), [makeconnection](makeconnection.html),
[makeLFO](makeLFO.html), [makerandom](makerandom.html),
[makeconverter](makeconverter.html), [makemonitor](makemonitor.html),
[makefilter](makefilter.html), [plottable](plottable.html)
