---
title: random()
layout: ref
---

## random

Return a random number between 0 and 1.

-----

### Synopsis

value = **random**()

-----

### Description

Call **random** to get a random number between 0 and 1, inclusive.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **random**. Otherwise, a seed of 1 will be
used.

There are no arguments to **random**.

-----

### Examples

``` 
   srand(1)
   for (i = 0; i < 10; i = i + 1) {
      randval = random() * 1000
      print(randval)
   }
```

prints 10 random numbers having values between 0 and 1000, inclusive.

The following complete CMIX script plays repeated notes of the same
pitch, sprayed randomly across the stereo field. This is easy to do,
because the *stereo\_loc* argument to most instruments has the same
range as the value returned by **random**.

``` 
   rtsetparams(48000, 2)
   load("WAVETABLE")

   control_rate(20000)   // short notes need high control rate
   ampenv = maketable("line", 1000, 0,0, 1,0, 10,1, 40,0)
   wavetable = maketable("wave", 1000, 1, 1, 1, 1, 1, 1, 0.5)

   srand(10)
   amp = 8000
   freq = 80
   dur = 0.04
   for (start = 0; start < 8; start = start + 0.11) {
      stereo_loc = random()
      WAVETABLE(start, dur, amp, freq, stereo_loc, wavetable)
   }
```

-----

### See Also

[irand](irand.html), [srand](srand.html), [trand](trand.html),
[rand](rand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
