---
title: get\_spray()
layout: ref
---

## get\_spray

Retrieve an integer from a spray table.

-----

### Synopsis

integer\_value = **get\_spray**(*spray\_table\_number*)

-----

### Description

Call **get\_spray** from a script to get a number from a spray table
initialized by [spray\_init](spray_init.html). The table contains
integers from zero to one less than the table size. Repeatedly calling
**get\_spray** reads the table in a random order, but in such a way that
no number appears twice before all the other numbers have appeared once.

-----

### Arguments

  - *spray\_table\_number* 
      
    The numeric ID for the spray table. You can have as many as 32 spray
    tables, numbered from 0 to 31.

-----

### Examples

This complete score plays two randomly generated twelve-tone rows, one
after the other, in an appropriately irritating way. Each pitch appears
exactly once during the first 12 notes, and once again during the second
12 notes.

``` 
   rtsetparams(44100, 1)
   load("WAVETABLE")

   amp = 20000
   ampenv = maketable("line", 1000, 0,0, 1,1, 10,0)

   /* make sawtooth wave */
   sawtable = maketable("wave", 1000, 1, 1/2, 1/3, 1/4, 1/5, 1/6, 1/7, 1/8, 1/9)

   seed = 0
   spray_init(0, 12, seed) /* make spray table 0 with 12 elements */

   for (i = 0; i < 24; i = i + 1) {
      val = get_spray(0)   /* access spray table 0 */
      pitch = 8 + (val * 0.01)  /* convert to oct.pc */
      st = i * 0.25
      WAVETABLE(st, dur = 0.25, amp*ampenv, pitch, 0, sawtable)
   }
```

  
  
This complete score shows how to make a sound pan randomly to specific
locations (rather than to just any random location). The sound will pan
to each location once, in a random order, then pan to each location once
more, in a random order, etc., for 10 seconds. Note that this does not
mean there will be no direct repetitions of a location, since this can
occur when starting to read the table for a second time, for example.
(The last value returned during the first read could be the value
returned at the start of the second read.)

``` 
   rtsetparams(44100, 2)
   load("WAVETABLE")

   amp = 20000
   ampenv = maketable("curve", 1000, 0,0,0, 1,1,-5, 30,0)

   sawtable = maketable("wave", 1000, 1, .3, .1)  /* dull sawtooth */

   seed = 1
   spray_init(0, 3, seed) /* make spray table 0 with 3 elements */

   /* make array containing 3 pan locations */
   panarr = { 0.0, 0.5, 1.0 }

   dur = 0.2
   pitch = 8.00
   for (st = 0; st < 10; st = st + 0.25) {
      /* use spray value as index into the pan array */
      index = get_spray(0)       /* access spray table 0 */
      pan = panarr[index]
      WAVETABLE(st, dur, amp*ampenv, pitch, pan, sawtable)
   }
```

-----

### See Also

[irand](irand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [rand](rand.html), [random](random.html),
[spray\_init](spray_init.html), [srand](srand.html), [irand](trand.html)
