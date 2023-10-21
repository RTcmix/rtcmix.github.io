---
title: print\_on()
layout: ref
---

## print\_on

Turn on RTcmix printing, and specify the kinds of information to see.

-----

### Synopsis

**print\_on**([*print\_level*])

-----

### Description

**print\_on()** turns on reporting of RTcmix scheduling, file
information, etc. (This corresponds to *print\_level* 5 in the table below.) 
A subsequent call to [print\_off](print_off.html) will
stop RTcmix printing.

Here are the values for the optional *print\_level* argument, which lets
you specify what kinds of information will print:

| *print\_level* | messages that print    |
|      :---:     | :---                   |
| 0              | fatal errors           |
| 1              | rterrors               |
| 2              | warn errors            |
| 3              | print and printf's     |
| 4              | advise notifications   |
| 5              | all the rest           |
| 6              | internal debug info    |

-----

### Examples

```cpp
   print_on()
```
This turns on printing at *print\_level* 5.

```
   print_on(3)
   printf("Print my name: %s\n", myname)
```

This prints the value of *myname*, but does not print all the
scheduling and other voluminous information.


-----

### See Also

[print](print.html), [printf](printf.html), [type](type.html),
[dumptable](dumptable.html), [set\_option](set_option.html#print),
[print\_off](print_off.html)
