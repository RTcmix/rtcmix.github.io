---
title: irand()
layout: ref
---

## irand

Return a random number within a specified range.

-----

### Synopsis

value = **irand**(\[*minimum*,\] *maximum*)

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **irand** to get a random floating-point number that falls within the
range specified by the arguments.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **irand**. Otherwise, a seed of 1 will be
used.

-----

### Arguments

  - *minimum* \[optional\], *maximum*  
      
    These arguments define the range within which falls the random value
    returned by **irand**. If *minimum* is not present, **irand** will
    return a value between 0.0 and *maximum*. The bounds are inclusive.

-----

### Examples

``` 
   srand(0)
   min = -30
   max = 10
   for (i = 0; i < 20; i = i + 1) {
      randval = irand(min, max)
      print(randval)
   }
```

-----

### See Also

[srand](srand.html), [trand](trand.html), [random](random.html),
[rand](rand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
