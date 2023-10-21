---
title: pickwrand()
layout: ref
---

## pickwrand

Return a weighted random choice from a set of numbers.

-----

### Synopsis

value = **pickwrand**(*number1*, *probability1* \[, *number2*,
*probability2*, ... *numberN*, *probabilityN* \])

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **pickwrand** to choose randomly among several numbers that you
specify, with a probability for each number.

It's a good idea to call [srand](srand.html) once to seed the random
number generator before using **pickwrand**. Otherwise, a seed of 1 will
be used.

-----

### Arguments

  - *number*  

  - *probability*  
      
    There can be as many *number*, *probability* pairs as you like, as
    long as there is at least one pair.
    
    A *probability* argument determines how likely it is that
    **pickwrand** will choose the corresponding *number* argument. The
    higher the *probability*, the more likely.
    
    The total probability is the sum of all the *probability* arguments.

-----

### RETURN VALUE

One of the *number* arguments to **pickwrand**, selected randomly in
accordance with the given probabilities.

-----

### Examples

```cpp
   srand(0)
   while (outskip < ending_time) {
      stereo_loc = pickwrand(0.0, 10,  0.5, 80,  1.0, 10)
      WAVETABLE(outskip, dur, amp, frequency, stereo_loc)
      outskip = outskip + 0.2
   }
```

plays [WAVETABLE](../instruments/WAVETABLE.html) notes, panning them in
accordance with the following probabilities: 10% of the notes will pan
to hard left, 10% of the notes will pan to hard right, and 80% of the
notes will pan to the center.

-----

### See Also

[irand](irand.html), [srand](srand.html), [trand](trand.html),
[random](random.html), [rand](rand.html), [pickrand](pickrand.html),
[spray\_init](spray_init.html), [get\_spray](get_spray.html),
[maketable("random")](maketable.html#random)
