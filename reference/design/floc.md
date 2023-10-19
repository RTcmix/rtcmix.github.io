---
title: floc()
layout: ref
---

### floc

*return a pointer to a function table slot*  

### Synopsis

### Description

**floc()** returns a pointer to the beginning of an array in a function
table slot created with the makegen scorefile command. For example: if
you issue the scorefile command

``` 
    makegen(4, 10, 1000, 1, 2, 3, 4)
```

and then in an instrument say

``` 
     float *f;

     f = floc(4);
```

  

the pointer f will be the address of the beginning of the array created
by makegen command. **floc()** issues an error message and terminates
the job if no function with that number has been generated.

### See Also

[fsize](fsize.html)
