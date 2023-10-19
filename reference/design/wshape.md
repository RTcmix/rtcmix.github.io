---
title: wshape()
layout: ref
---

### wshape

*waveshaping signal-processor*  
  
**whsape** takes a sample, a pointer to a transfer function table (see
[floc](floc.html) to use a function table slot) and the length of the
transfer function table (see [fsize](fsize.html)) and returns a
'waveshaped' (table-lookup) version of the sample.

From the source code:

-----

float wshape(float x, float \*f, int len)  
x = sample  
f = pointer to the transfer function table  
len = length of the transfer function table

-----

Example of use:

``` 

float insample, outsample, *transfunc, int len;

transfunc = floc(2); // use function table slot # 2 from scorefile
len = fsize(2);

 ...

for (i = 0; i < framesToRun(); i++) {
   insample = someinputsample;

   outsample = wshape(insample, transfunc, len);

 ...
}
```
