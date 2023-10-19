---
title: table()
layout: ref
---

### table/tablei/tableset

*initialize and access a function table slot*  
  

### Synopsis

### Description

These units will return a value from the *\*array* as a function of
*nsample*, which is the number of the current sample, counting up from
0. The array *\*tab* must be dimensioned at 2 in the calling routine. It
is used for bookkeeping purposes. **tableset** initializes the values in
the *\*tab* array as functions of *dur*, which is the expected duration
of the note, and *size*, which is the number of locations in the array
to be sampled. The values in *\*tab* are left untouched by **table** and
**tablei** so they can be called to look at any tables of the same size.
**tablei** is an interpolating version of **table**.

The value of these units is that they do not need to be called on every
sample and can thus be used in control loops to create amplitude curves
etc. For example:

``` 
          long nsample,totalsamps;
          short countdown,loopsize,size;
          float amp,array[???],tab[2],dur;
          .
          .
          tableset(dur,size,tab);
          .
          .
          for(i = 0; i < framesToRun(); i++) {
                                   /* main performance loop */
               if(!countdown--) {
                                   /* periodic control loop */
                    amp = table(currentFrame(), array, tab);
                    countdown = loopsize;
               }
               .
               .
          }
          .
          .
          .
```

If *nsample* points to a time greater than than that declared by *dur*
the last value in the array will be returned.

### See Also

[env](evp.html), [Ooscili](Ooscil.html),

### Bugs

Not sure what will happen if negative times or samples are passed in
argument list.
