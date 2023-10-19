---
title: REVMIX()
layout: ref
---

## REVMIX

Play an input soundfile backwards.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**REVMIX**(outsk, insk, dur, AMP\[, inputchan, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = input channel [optional; default is 0]
   p5 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (amplitude) and p5 (pan) can receive dynamic updates from a table or
   real-time control source.

   Author: Ivica "Ico" Bukvic, 27 May 2000
   (with John Gibson )
   rev. for v4.0, JGG, 7/9/04
```

  

-----

  
**REVMIX** plays a selected channel of the input file backward for the
specified duration, starting at the input start time.

### Usage Notes

If you specify a duration that would result in an attempt to read before
the start of the input file, **REVMIX** will shorten the note to prevent
this.

Note that you can't use this instrument with a real-time input
(microphone or aux bus), only with input from a sound file. (That's
because the input start time of an inst taking real-time input must be
zero, but this instrument reads backward from its inskip.) Duh...

The output of **REVMIX** may be mono or stereo

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("REVMIX")

   rtinput("mysound.aif")

   // do both channels of a stereo input file
   REVMIX(outskip=0, inskip=5, dur=3, amp=1, inchan=0, loc=0.0)
   REVMIX(outskip=0, inskip=5, dur=3, amp=1, inchan=1, loc=1.0)
```

  

-----

### See Also

[MIX](MIX.html), [STEREO](STEREO.html), [PAN](PAN.html)
