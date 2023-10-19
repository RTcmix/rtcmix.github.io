---
title: fsize()
layout: ref
---

### fsize

*returns the array size of a function table slot*  
  

### Synopsis

### Description

**fsize** returns the size of an array in a function table slot created
with makegen. For example: if you issue the command:

``` 
    makegen(4, 10, 1000, 1, 2, 3, 4)
```

and then in an instrument say:

``` 
     int size;

     size = fsize(4);
```

the integer *size* will be the size of the array in in the function
table slot (in floating-point words) as specified in the third argument
of the makegen scorefile command. *fsize* issues an error message and
terminates the job if no function with that number has been generated.

### See Also

[floc](floc.html)
