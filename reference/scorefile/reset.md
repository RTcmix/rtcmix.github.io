---
title: reset()
layout: ref
---

## reset / control\_rate

Set the update rate for control functions and envelopes.

-----

### Synopsis

**control\_rate**(*update\_rate*)

**reset**(*update\_rate*)  

-----

### Description

For instruments that are designed to use *resetval* in updating control
functions and envelopes (pfields), **reset** determines how many
times/second the control function/envelope is 'sampled'. For example,

``` 
   reset(48000)
```

will cause a control function to be sampled 48000 times/second (or
once-every-sample for a standard 48k sampling-rate sound).

Why is this desired? Often it is not necessary to update control
functions for every sample, and an instrument can run much more
efficiently by choosing a lower sampling rate for control functions.

The default rate is 1000 times per second.

Most, but not all, RTcmix instrument control parameters will respond to
the **reset** command. Exceptions for particular controls are usually
noted in the instrument documentation.

If the reset rate is made too slow, a "stair-stepping" or "zippering"
distortion effect can occur in control function access, especially
noticeable in amplitude envelopes.

-----

### Arguments

  - *update\_rate*  
    Any number, floating point or integer represeting how many
    times/second the control/envelope (pfield) information will be
    updated during note execution.  Values greater than the current
    sampling rate will produce unpredictable results.

-----

### Examples

``` 
   reset(10000)
   control_rate(22050)
```

-----

### See Also

[rtsetparams](rtsetparams.html)
