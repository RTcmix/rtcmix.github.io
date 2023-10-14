---
title: spray\_init()
layout: ref
---

## spray\_init

Initialize a random integer spray can.

-----

### Synopsis

**spray\_init**(*spray\_table\_number*, *spray\_table\_size*,
*random\_seed*)

-----

### Description

Call **spray\_init** from a script to set up a random integer spray can.
The idea is that you create a table of integers and then use
[get\_spray](get_spray.html) to retrieve them. The table contains
integers from zero to one less than the table size. (Another way to look
at it is that each element takes its index as its value.) Repeatedly
calling [get\_spray](get_spray.html) reads the table randomly, but in
such a way that no number appears twice before all the other numbers
have appeared once.

-----

### Arguments

  - <span id="item_spray_table_number">*spray\_table\_number*</span>  
      
    The numeric ID for the spray table. You can have as many as 32
    tables, numbered from 0 to 31. Note that these are separate entities
    from tables created using the [maketable](maketable.html) scorefile
    command.

  - <span id="item_spray_table_size">*spray\_table\_size*</span>  
      
    The table can have as many as 512 values.

  - <span id="item_random_seed">*random\_seed*</span>  
      
    An integer to seed the random number generator. Note that this
    re-seeds the same random number generator used by [rand](rand.html),
    [random](random.html), [irand](irand.html), etc.

-----

### Examples

``` 
   seed = 78
   spray_init(3, 7, seed) // initialize table 3 with 7 elements

   for (i = 0; i < 21; i = i+1) {
      val = get_spray(3)
      print(val)
   }
```

-----

### See Also

[get\_spray](get_spray.html), [irand](irand.html),
[pickrand](pickrand.html), [pickwrand](pickwrand.html),
[rand](rand.html), [random](random.html), [srand](srand.html),
[irand](trand.html)
