---
title: pickrand()
layout: ref
---

## pickrand

Return a random choice from a specified set of numbers.

-----

### Synopsis

value = **pickrand**(*number1* \[, *number2*, ... *numberN* \])

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **pickrand** to choose randomly among several numbers that you
specify as its arguments.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **pickrand**. Otherwise, a seed of 1 will
be used.

-----

### Arguments

  - *number1, number2, ... numberN*  
      
    There can be any number of numerical arguments to **pickrand**, as
    long as there is at least one.

-----

### RETURN VALUE

One of the arguments to **pickrand**, selected randomly.

-----

### Examples

``` 
   srand(1)
   for (i = 0; i < 10; i = i + 1) {
      pitch = pickrand(8.00, 8.02, 8.04, 8.05, 8.07, 8.09, 8.11)
      print(pitch)
   }
```

prints 10 pitches from a C major scale, selected randomly. There's no
guarantee that this will print each of the arguments at least once. Use
the spray mechanism for that ([spray\_init](spray_init.html),
[get\_spray](get_spray.html)).

-----

### See Also

[irand](irand.html), [srand](srand.html), [trand](trand.html),
[random](random.html), [rand](rand.html), [pickwrand](pickwrand.html),
[spray\_init](spray_init.html), [get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
