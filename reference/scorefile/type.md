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

  - [float](Minc.html#float)
  - [string](Minc.html#string)
  - [handle](Minc.html#handle)
  - [list (array)](Minc.html#list)
  - [struct](Minc.html#struct)
  - [map](Minc.html#map)
  - [mfunction](Minc.html#mfunction)
  - void (only used for error returns)

-----

### Arguments

  - *somevar*  
    Any valid [Minc](Minc.html) variable.

-----

### Examples

```cpp
   datatype= type(arbitrary_var)
   print(datatype)
```

-----

### See Also

[Minc](Minc.html), [len](len.html), [print](print.html), [printf](printf.html),
[stringify](stringify.html)
