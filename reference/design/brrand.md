---
title: brrand()
layout: ref
---

### brrand/sbrrand

*pseudo-random number generator and initialization*  
  
This runs a block-computing version of the [rrand](rrand.html) random
number generator and [srrand](rrand.html) seed/initiliaztion program.

From the source code:

-----

    /* block version of rrand */
    /* a modification of unix rand() to return floating point values between
       + and - 1. */
    
    sbrrand(unsigned int x)
    brrand(float amp, float *a, int j)
