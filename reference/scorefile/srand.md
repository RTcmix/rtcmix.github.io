---
title: srand()
layout: ref
---

## srand

Seed the random number generator.

-----

### Synopsis

**srand**(\[ *seed* \])

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **srand** to seed RTcmix's built-in random number generator before
using one of the random functions.

You can call **srand** more than once in a script, if you want to
restart the random number generator.

If you don't use **srand**, RTcmix uses 1 as the random seed.

-----

### Arguments

  - <span id="item_seed">*seed*</span>  
      
    The seed can be any integer. Starting with a specific seed causes a
    random function to generate the same series of numbers every time
    you run the script. (Though it's possible that different platforms
    will give different results from the same script.)
    
    If not present, then the seed is taken from the system microsecond
    counter. This means that the series of numbers generated by random
    functions will be different every time you run the script.

-----

### Examples

``` 
   srand(1)
   value = rand()
   print(value)
```

prints 0.02770996 every time you run the script.

``` 
   srand()
   value = rand()
   print(value)
```

prints a different number every time you run the script, because the
seed is never the same (since it comes from the microsecond counter).

``` 
   srand(777)
   for (start = 0; start < 10; start = start + 0.5) {
      frequency = (random() * 200) + 300
      WAVETABLE(start, dur, amp, frequency)
   }
```

plays a series of notes with frequencies between 300 and 500 Hz. This
series will be the same every time you run the script.

-----

### See Also

[irand](irand.html), [trand](trand.html), [random](random.html),
[rand](rand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)