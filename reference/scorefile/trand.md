---
title: trand()
layout: ref
---

## trand

Return a random integer within a specified range.

-----

### Synopsis

value = **trand**(\[*minimum*,\] *maximum*)

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **trand** to get a random integer that falls within the range
specified by the arguments.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **trand**. Otherwise, a seed of 1 will be
used.

-----

### Arguments

  - <span id="item_minimum_maximum">*minimum*\[optional\], *maximum*</span>  
      
    These arguments define the range within which falls the random value
    returned by **trand**. Whether the range is inclusive of min or max
    depends on whether these values are negative:
    
      - The range is inclusive of min if min \>= 0.
      - The range is inclusive of max if max \<= 0.
    
    For example:
    
      - min = 0, max = 10 =\> return integer between 0 and 9 (inclusive)
      - min = -10, max = 0 =\> return integer between -9 and 0
        (inclusive)
      - min = -10, max = 10 =\> return integer between -9 and 9
        (inclusive)
      - min = 5, max = 10 =\> return integer between 5 and 9 (inclusive)
    
    If only one arg is present, it is max, and min is set to zero.
    
    If max \< min, the two arguments are exchanged so that they are in
    ascending numerical order (then the rules above are applied to the
    exchanged min and max values).

-----

### Examples

```cpp
   srand(0)
   min = -30
   max = 10
   for (i = 0; i < 20; i = i + 1) {
      randval = trand(min, max)
      print(randval)
   }
```

another useful example:

```cpp
   array = { 1, 2, 3, 4, 5, 6, 7 }
   len_array = len(array)
   for (i = 0; i < 14; i += 1) {
      val = array[trand(len_array)]
   }
```

-----

### See Also

[irand](irand.html), [srand](srand.html), [random](random.html),
[rand](rand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
