---
title: type()
layout: ref
---

## type

Return the Minc data-type of the argument.

-----

### Synopsis

strval = **type**(*somevar*)

-----

### Description

**type** will return (as a string) the data-type of its argument. At
present the following are valid [Minc](Minc.html) data-types:

  - void
  - float
  - string
  - handle
  - list
  - struct
  - map
  - function
  - instrument

-----

### Arguments

  - *somevar*  
    Any valid [Minc](Minc.html) variable.

-----

### Examples

``` 
   datatype= type(arbitrary_var)
   print(datatype)
```

-----

### See Also

[len](len.html), [print](print.html), [printf](printf.html),
[stringify](stringify.html)
