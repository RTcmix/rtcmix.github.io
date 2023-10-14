---
title: makerandom
layout: ref
---

## makerandom

set up a periodic random-number generator for control
purposes, using a *pfield-handle* connection to an Instrument parameter

-----

### Synopsis

pfield-handle = **makerandom**(*"type"*, *frequency*, *min*, *max*\[,
seed\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**makerandom** returns a *pfield-handle* that will deliver data
periodically from an internal RTcmix random-number generator. The period
is determined by the *frequency* argument, and the random numbers will
lie within the range set by the *min* and *max* arguments. The
random-numbers will be generated according the the random-number
distribution specified in the *"type"* string argument. These
distributions are identical to the ones decribed in the [maketable
"random"](maketable.html#random) table construction command. The
optional *seed* argument allows the user to specify a particular seed
value for the psuedorandom algorithm used by the random-number
generator. The active control rate (set via [control\_rate](reset.html))
must be the same for makerandom and the instrument that uses the
makerandom pfield-handle.

Many of the arguments for **makerandom** may themselves also be
pfield-handles.

-----

### Arguments

  - <span id="type" class="internallink">*type*</span>  
      
    This string value (i.e. enclosed in "double quotes" in the
    scorefile) determines the type of random-number distribution to use
    in generating the periodic values. The different types currently
    supported are:
    *"even"/"linear"* -- randomly select numbers between *min* and
    *max*.  
      
    *"low"* -- randomly select numbers between *min* and *max*, but with
    a higher probability of choosing numbers nearer the *min* value.  
      
    *"high"* -- randomly select numbers between *min* and *max*, but
    with a higher probability of choosing numbers nearer the *max*
    value.  
      
    *"triangle"* -- randomly select numbers between *min* and *max*, but
    with the probability of choosing a value determined by a triangular
    curve with the apex at the midpoint between the *min* and *max*
    values. In other words, numbers near either *min* or *max* will have
    a very low probability of being generated, but numbers half-way
    between will have a high probability of being chosen.  
      
    *"gaussian"* -- randomly select numbers between *min* and *max*, but
    with the probability of choosing a value determined using a
    [Gaussian](http://mathworld.wolfram.com/NormalDistribution.html)
    ('normal'; 'bell curve') probability distribution with the apex at
    the midpoint between the *min* and *max* values. Similar to how the
    *"triangle"* specifier operates.  
      
    *"cauchy"* -- randomly select numbers between *min* and *max*, but
    with the probability of choosing a value determined using a
    [Cauchy](http://www.itl.nist.gov/div898/handbook/eda/section3/eda3663.htm)
    function with the apex at the midpoint between the *min* and *max*
    values. Similar to how the *"triangle"* and *"gaussian"* specifiers
    operate.  
      
    *"prob"* -- Mara Helmuth's configurable probability distribution.
    This has a slightly different syntax:
    <!-- end list -->

<span id="frequency" class="internallink">*frequency*</span>  

The frequency (in Hz) determines the rate at which random values will be
generetated through the *pfield-handle* This should be less than the
[reset](reset.html) rate.

<span id="min" class="internallink">*min*</span>  

<span id="max" class="internallink">*max*</span>  

These two arguments define the range of random values that will be
produced by the random-number generator through the *pfield-handle*.
*min* will set the minimum value of the range, and *max* will set the
upper bound.

<span id="seed" class="internallink">*seed*</span>  

This optional argument sets the 'seed' (or initial value) for the
pseudorandom number algorithm used by RTcmix. Each seed value will
generate a unique sequence of "random" numbers. If the *seed* argument
is 0, then the 'seed' for the psuedorandom number algorithm comes from
the microsecond system clock, otherwise the value of *seed* is used as
the 'seed'. If no seed argument is present, the 'seed' used is 0 (i.e.
the 'seed' will come from the system clock).

-----

### Examples

``` 
   pitch1 = makerandom("low", 10, 8.00, 8.11)
   pitch2 = makerandom("high", 15, 8.00, 8.11)
   wave = maketable("wave", 1000, 1.0, 0.2, 0.1)
   WAVETABLE(0, 4.9, 15000, pitch1, 0.0, wave)
   WAVETABLE(0, 4.9, 15000, pitch2, 1.0, wave)
```

This scorefile uses two random-number PField generators, one operating
at 10 Hz (10 values/second) and the other at 15 Hz (15 values/second) to
control the pitch of two [WAVETABLE](../instruments/WAVETABLE.html)
notes, one in the left channel and one in the right. The pitch (in
octave.pitch-class notation) is coming directly from the random-number
*pfield-handles, pitch1* and *pitch2*. The range of these random-number
generators is set to produce values within the octave 8.00 to 8.11
(middle "C" to the "B" above middle "C").

-----

### See Also

[maketable](maketable.html), [makeconnection](makeconnection.html),
[makeLFO](makeLFO.html), [makefilter](makefilter.html),
[makeconverter](makeconverter.html), [makemonitor](makemonitor.html),
[irand](irand.html), [srand](srand.html), [trand](trand.html),
[random](random.html), [rand](rand.html), [pickrand](pickrand.html),
[pickwrand](pickwrand.html), [spray\_init](spray_init.html),
[get\_spray](get_spray.html),