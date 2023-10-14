---
title: print\_off()
layout: ref
---

## print\_off

Turn off RTcmix printing.

-----

### Synopsis

**print\_off**()

-----

### Description

**print\_off()** turns off reporting of RTcmix scheduling, file
information, etc. A subsequent call to [print\_on](print_on.html) will
restore RTcmix printing. This can often speed up RTcmix execution,
especially when large numbers of notes are being scheduled.

-----

### Examples

``` 
   print_off()
```

-----

### See Also

[print](print.html), [printf](printf.html), [type](type.html),
[dumptable](dumptable.html), [set\_option](set_option.html#print),
[print\_on](print_on.html)
