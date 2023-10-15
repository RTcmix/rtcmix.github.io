---
title: makefilter
layout: ref
---

## makefilter

Create a "filter" to alter PField control data, reading
from one *pfield-handle* or *table-handle* and passing the altered data
through another *pfield-handle*.

-----

### Synopsis

pfield = **makefilter**(*input\_handle*, *"filter\_type"*, *arg1, arg2,
...*)

Parameters inside the \[brackets\] are optional.

-----

### Description

**makefilter** returns a *pfield-handle* that will deliver data by
applying an operation defined by *"filter\_type"* to the data coming
through the *input-handle* variable. The *input\_handle* variable can be
any of the PField-derived variables in RTcmix, such as a *table-handle*
(see [maketable](maketable.html)) or another *pfield-handle* (see
[makeconnection](makeconnection.html), [makeLFO](makeLFO.html) or
[makerandom](makerandom.html)). The arguments vary depending on each
*[filter\_type](#filter_types)*.

Many of the arguments for **makefilter** may themselves also be
pfield-handles.

-----

### Arguments

  - *input\_handle*  
     
    This is *pfield-handle* variable referring to a data stream,
    possibly from an external device or interface or an internal PField
    data-generating process.

  - *filter\_type*  
     
    This string value (i.e. enclosed in "double quotes" in the
    scorefile) determines what kind of 'filter' will be used to
    transform the values coming through the *input\_handle*. The
    *pfield-handle* returned from the **makefilter** command will refer
    to this transformed data stream.

  - *arg1, arg2, ...*  
      
    The arguments that are relevant for each filter type. These will
    determine how the particular filter operates to transform the data.
    See documentation for each type below.

-----

## <span id="filter_types" class="internallink">Filter Types</span>

  - <span id="clip" class="internallink">*clip*</span>  
      
    The *clip* filter will This limits the incoming pfield values to the
    range defined by min and max, which both can be dynamic. The syntax is:

    ```
    pfield = makefilter(input_pfield, "clip", min[, max])
    ```

    Data coming through the *input\_pfield* variable less then the value
    *min* will be reset to the value *min*. Similarly, data greater than
    the value *max* (if present) will be set to *max*.

  - <span id="constrain" class="internallink">*constrain*</span>  
      
    The *constrain* filter works with two data streams, one being an
    incoming set of pfield values (as with all the *makefilter* filter
    types), and the other being a *table\_handle* p-field referring to
    an existing table of values. The syntax is:

    ```
    pfield = makefilter(input_pfield, "constrain", table_handle, strength)
    ```

    For each value coming through the *input\_pfield* variable, this
    filter will return the nearest numerical value in the
    *table\_handle* table. The *strength* parameter (from 0 to 1) is a
    measure of how closely the filter constrains the pfield to the table
    value. A *strength* value of 0 means no constraint (i.e., you get
    back the original pfield value); a *strength* of 1 means you get the
    table value; a *strength* of 0.5 means you get a value midway
    between the pfield and table values. *strength* can be time-varying.

  - <span id="delay" class="internallink">*delay*</span>  
      
    The *delay* filter introduces a time delay into the data stream
    coming through the *input\_pfield* variable. The syntax is:

    ```
    pfield = makefilter(input_pfield, "delay", time)
    ```

    *time* is the length of the delay (in seconds). The delay is
    relative to when the data appears through the *input\_pfield*.

  - <span id="fitrange" class="internallink">*fitrange*</span>  
      
    The *fitrange* filter will take data in the range \[0.0, 1.0\] or
    \[-1.0, 1.0\] and expand it to a specified range. The syntax is:

    ```
    pfield = makefilter(input_pfield, "fitrange", min, max[, "bipolar"])
    ```

    Data coming through the *input\_pfield* variable in the range \[0.0,
    1.0\] will be expanded and transformed to fit the range \[*min,
    max*\]. When the input data is 0.0, the output will be *min*. Input
    data at 1.0 will output *max*. If the optional *"bipolar"* string
    argument is given, then the *fitrange* filter assumes that the input
    data will be in the \[-1.0, 1.0\] range &mdash; when the input is -1.0,
    then *min* will be the output; input of 1.0 will yield *max*.

  - <span id="invert" class="internallink">*invert*</span>  
      
    The *invert* filter will take data from an input p-field and
    invert the values, i.e. all values are mirrored around a center
    value. The syntax is:

    ```
    pfield = makefilter(input_pfield, "invert"[, center])
    ```

    By default, the center value (y-axis center of symmetry) is a point
    halfway between the min and max table values. If the optional second
    *center* argument is present, this will be used as the vertical
    center of symmetry.

  - <span id="map" class="internallink">*map*</span>  
      
    The *map* filter sets up a 'transfer function' for data coming
    through an input p-field. The syntax is:

    ```
    pfield = makefilter(input_pfield, "map", transfer_table[, inputMin, inputMax])
    ```

    The operation of the *map* filter is a simple table-lookup transfer
    function. The transfer function itself is set using the
    *transfer\_table* table-handle (see [maketable](maketable.html) for
    how to create these tables). Values then coming in through the
    *input\_pfield* parameter are treated as X-locations in the
    *transfer\_table*, and the corresponding Y-value for a given
    X-location will be returned through the *pfield* variable. The input
    data stream can be scaled to match the X-range of the
    *transfer\_table* table using the optional *inputMin* and *inputMax*
    parameters.

  - <span id="quantize" class="internallink">*quantize*</span>  
      
    The *quantize* filter will transform data coming through the
    *input\_pfield* variable by shifting each input value to an integer
    multiple of the *quantize\_value*. The syntax is:

    ```
    pfield = makefilter(input_pfield, "quantize", quantize_value)
    ```

    All data values coming through the *input\_pfield* will be rounded
    to the nearest integer multiple (both positive and negative) of the
    *quantize\_value* and then output through the data stream associated
    with the *pfield* variable. A value exactly 0.5 between two
    successive multiples of *quantize\_value* will be rounded to the
    upper multiple. This is the inverse of the interpolation procedure
    used in the *"smooth"* filter.

  - <span id="smooth" class="internallink">*smooth*</span>  
      
    The *smooth* filter type will linearly interpolate values coming
    through the *input\_pfield* variable depending on the rate set by
    *lag*. This means that large jumps in input will be 'smoothed' into
    a series of smaller value jumps in the output. The syntax is:

    ```
    pfield = makefilter(input_pfield, "smooth", lag[, initial_value])
    ```

    The filter operates by applying a one-pole linear filter to the input
    values. *lag* is a percentage that determines how quickly (i.e. the
    number of interpolation steps) the filter will jump from one value
    to the next for a given input sequence. By default, it will start
    from 0 when initialized. Set the optional *initial value* value to
    prevent a swoop to the *input pfield* value at the start.

-----

### Examples

``` 
rpfield = makerandom("even", 7.0, 0.0, 100.0)
smpfield = makefilter("smooth", 20)
qpfield = makefilter("quantize", 20)
rpfield = makefilter("fitrange", 0.0, 1.0)
```

The [makerandom](makerandom.html) scorefile command will output random
numbers between 0.0 and 100.0 at a rate of 7 times/second (7.0 Hz).
Three different **makefilter** commands modify that data. Associated
with the *smpfield* variable will be data that is 'smoothed' by 20% --
large jumps in the random values will be "filled in" by interpolated
values. The *qpfield* stream will deliver data that consists of the
closest integer multiple of 20 (the output will be \[0.0, 20.0, 40.0,
60.0, 80.0, 100.0\] depending on the value of the input). *rpfield* will
take the \[0.0, 100.0\] range and scale it to fit between 0.0 and 1.0.

``` 
table = maketable("literal", "nonorm", 0, 8.00, 8.02, 8.04, 8.05, 8.07)
rpfield = makerandom("even", 10.0, 8.00, 8.07)
pitches = makefilter(rpfield, "constrain", table, 1.0)
```

This will take the random numbers (generated 10 times each second)
coming through *rpfield* and cause them to generate one of the specific
values in the *table* set of values. The result will be assigned through
the p-field variable *pitches*

``` 
table = maketable("expbrk", 1000, 0.0001, 1000, 1.0)
newpfield = makefilter("invert", table)
```

The p-field *newpfield* will be associated with data containing a convex
version of the concave exponential curve (or maybe vice-versa) created
using the original *"expbrk"* table-type.

-----

### See Also

[maketable](maketable.html), [modtable](modtable.html),
[makeconnection](makeconnection.html), [makeLFO](makeLFO.html),
[makerandom](makerandom.html), [makeconverter](makeconverter.html),
[makemonitor](makemonitor.html)
