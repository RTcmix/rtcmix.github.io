---
title: makemonitor()
layout: ref
---

## makemonitor

Create and use a window to display PField data from
*pfield-handle* or *table-handle* (or similar derived PField) variable,
or capture PField data into a file.

-----

### Synopsis

pfield = **makemonitor**(*input\_handle*, *"monitor\_type"*, *arg1,
arg2, ...*)

-----

### Description

**makemonitor** is a utility command used to set up a mechanism for
viewing or recording data from a PField variable, the *input\_handle*.
The *"monitor\_type"* string determines which 'monitoring' operation
will be performed. The arguments vary depending on each
*[monitor\_type](#monitor_types)*.

-----

### Arguments

  - *input\_handle*  
      
    This is *pfield-handle* variable referring to a data stream,
    possibly from an external device or interface or an internal PField
    data-generating process.

  - *monitor\_type*  
      
    This string value (i.e., enclosed in "double quotes" in the
    scorefile) determines what kind of monitor will be used record or
    view the values coming through the *input\_handle*. The
    *pfield-handle* returned from the **makemonitor** command will pass
    the data from the *input\_handle* data stream unchanged.

  - *arg1, arg2, ...*  
      
    The arguments that are relevant for each monitor type. See
    documentation for each type below.

-----

## <span id="monitor_types" class="internallink">Monitor Types</span>

  - <span id="datafile" class="internallink">*datafile*</span>  
      
    This will record the values coming through the *input\_handle*
    variable into a file. The syntax is:
    
    This will open the file *filename* for writing, attaching an
    appropriate header containing information relevant to the
    interpretation of the file data. This header can be read by the
    RTcmix [makeconnection](makeconnection.html#datafile) scorefile
    command, or an external program can access the header fields. As the
    RTcmix score executes, data coming through the *input\_pfield*
    variable will be stored into the file. The rate for writing data is
    the control reset rate (default 1000 times/second, this is set in
    the score by the RTcmix [reset](reset.html) scorefile command), or
    it can be set independently using the optional *filerate* parameter.
    The default data format for the file is to store information in
    floating-point form (*"float"*). This can be set using the optional
    *format* parameter. Valid formats are *"double", "float", "int64",
    "int32", "int16"* or "byte".
    
    RTcmix also has a built-in safeguard to prevent overwriting existing
    files. This feature is enabled/disabled using the
    [set\_option](set_option.html#clobber) command. The default is to
    not allow overwriting of a file.
    
    Note that using a *filerate* greater than the scorefile control
    reset rate will result in redundant data. Large values for
    *filerate* or the control reset rate are inefficient and will impact
    RTcmix performance.

  - <span id="display" class="internallink">*display*</span>  
      
    This monitor type is used to create and use a window to display
    streaming PField data from a *pfield-handle* or *table-handle* (or
    similar derived PField) variable. The syntax is:
    
    The *input\_pfield* is the PField stream that will be displayed in
    the window. This can be any PField-derived variable, such as a
    *pfield-handle* or *table-handle*. The optional arguments *prefix*,
    *units* and *precision* determine how the data will be printed in
    the widow. *prefix* is a string argument that will specify the label
    to display in the window for the *input\_pfield* variable. If
    *prefix* is missing or is an empty string ("") then no label will be
    printed in the window. *units* is an optional string argument to set
    the 'units' (i.e. "Hz" or "MIDI note") to appear with the
    *input\_pfield* data displayed. This option is only available if the
    *prefix* argument has been set. The *precision* is the number of
    digits after the decimal point to display in the window.

-----

### Examples

```cpp
   xval = makeconnection("mouse", "x", 0, 100, 50, lag=50)
   makemonitor(xval, "display", "x-value", "pts")
```

This set of scorefile commands should create two different windows, one
from the first [makeconnection](makeconnection.html) command for
capturing mouse position, and the other (smaller) window used to display
the x-axis location of the mouse.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makeconnection](makeconnection.html), [makeLFO](makeLFO.html),
[makerandom](makerandom.html), [makeconverter](makeconverter.html),
[makemonitor](makefilter.html)
