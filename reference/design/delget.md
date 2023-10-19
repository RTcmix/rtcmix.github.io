---
title: delget()
layout: ref
---

### delget/delset/delput/dliget

*initialize and use delay lines*  
  

### Synopsis

### Description

These units are used to store and fetch samples from a delay line.
**delset()** is the initialization unit for **delget(), delput(),** and
**dliget()**. *array* is an array of floats which must be at least
*maxlength* words long, where this number is the maximum size you expect
to use (sampling rate \* maximum delay in seconds). *delvals* is an
array of two integers used for bookkeeping purposes. **delput()** stores
*signal* in *array*, and **delget()** fetches a sample from the pipeline
where *wai* is the age of the sample in seconds. E.g. if *wait* is equal
to .5 at a sampling rate of 30000, **delget()** will look back 15000
samples. *array* must be dimensioned appropriately. Naturally,
**delput()** must be called before **delget()**, **dliget()** is
identical with **delget()** except that it will interpolate linearly
between samples according to the fractional part of *wait*.
