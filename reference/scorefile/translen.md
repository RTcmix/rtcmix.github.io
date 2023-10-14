---
title: translen()
layout: ref
---

## translen

Return the projected duration of a transposed (interpolated) sound.

-----

### Synopsis

val = **translen**(*original\_length, tranposition\_interval*)

-----

### Description

**translen** returns the duration of a transposed (pitch-shifting via
interpolation &mdash; see the [TRANS](../instruments/TRANS.html) or
[TRANS3](../instruments/TRANS3.html) instruments) sound given the
*original\_length* (in seconds) and *transposition\_interval* (in
octave.pitch-class notation &mdash; i.e., -0.07 will shift downwards by a
perfect fifth).

For example, if you transpose a sound down an octave (-1.00) with
[TRANS](../instruments/TRANS.html), it will create two seconds of output
for every one second of input. The duration that you pass to
[TRANS](../instruments/TRANS.html) is the duration of the output. You
use **translen** if you want to consume a particular duration of the
input sound, regardless of the duration of the resulting output. Say you
had a one-second recording of someone saying "RTcmix rocks\!" If you
want to transpose this down an octave, you could do it two ways:

``` 
   duration = 1
   TRANS(start, inskip, duration, amp, transp)
```

...or...

``` 
   duration = 1
   TRANS(start, inskip, translen(duration, transp), amp, transp)
```

The first example would give you one second of output, but it would
truncate the sentence in the middle, after a half second of sound is
consumed. The second example gives you the entire sentence, with the
**translen** call telling [TRANS](../instruments/TRANS.html) to run for
two seconds in order to consume the entire input sound.

If you were to transpose the sound up an octave instead, then passing a
duration of one to [TRANS](../instruments/TRANS.html) would give you a
one-second sound, but the second half of that sound would be silent,
since it takes only a half second to play the entire sentence when
transposed up an octave. Using **translen** here with a one-second
duration argument would cause [TRANS](../instruments/TRANS.html) to run
for a half second, ensuring that any envelope table will span the
sentence, rather than having the sentence end in the middle of the
envelope, causing a click.

-----

### Arguments

  - <span id="original_length">*original\_length*</span>  
      
    The original (untransposed) length in seconds of the input sound.

  - <span id="tranposition_interval">*tranposition\_interval*</span>  
      
    The amount of transposition in octave.pitch-class notation (see the
    [pchcps](pchcps.html) convertor for an explanation of this
    notation).

-----

### Examples

``` 
   newlength = translen(7.8, -0.05)
```

(and see the [DESCRIPTION](#DESCRIPTION) section above for additional
examples.)

-----

### See Also

[DUR](DUR.html), [filedur](filedur.html),
[TRANS](../instruments/TRANS.html)
