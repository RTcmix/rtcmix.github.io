---
title: rand()
layout: ref
---

## rand

Return a random number between -1 and 1.

-----

### Synopsis

value = **rand**()

-----

### Description

Call **rand** to get a random number between -1 and 1, inclusive.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **rand**. Otherwise, a seed of 1 will be
used.

There are no arguments to **rand**.

-----

### Examples

``` 
   srand(1)
   for (i = 0; i < 20; i = i + 1) {
      randval = rand() * 10
      print(randval)
   }
```

prints 20 random numbers having values between -10 and 10.

The following complete CMIX script plays repeated notes of the same
pitch. The attack times of the notes vary randomly from an even grid
whose lines are spaced 0.14 seconds apart, and the amplitudes range from
400 to 3600.

``` 
   rtsetparams(48000, 1)
   load("WAVETABLE")

   ampenv = maketable("line", 1000, 0,0, 1,0, 20,1, 40,0)
   wavetable = maketable("wave", 1000, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1)

   srand(0)
   smear = 0.12
   amp = 2000
   dur = 0.06
   for (start = smear; start < 10; start = start + 0.14) {
      randval = rand()
      st = start + (randval * smear)
      randval = rand()
      a = amp + (randval * (amp * 0.8))
      WAVETABLE(st, dur, a*ampenv, freq = 1200, 0, wavetable)
   }
```

The basic strategy is to multiply the return value of **rand**, which
falls between -1 and 1, by some factor that modifies this range. You
could randomize the frequency in the same manner.

-----

### See Also

[irand](irand.html), [srand](srand.html), [trand](trand.html),
[random](random.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
