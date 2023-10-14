---
title: stringify()
layout: ref
---

## stringify

Return pointer to a string argument.

-----

### Synopsis

val = **stringify**(*"somestring"*)

-----

### Description

**stringify** returns a pointer to the string argument (*"somestring"*)
suitable for storing in RTcmix variables.

-----

### Arguments

  - <span id="item_somestring">*"somestring"*</span>  
      
    The string to be converted to an RTcmix pointer.

-----

### Examples

    rtptr = stringify("hey there!")

-----

### See Also

[print](print.html), [printf](printf.html), [type](type.html),
[len](len.html)
