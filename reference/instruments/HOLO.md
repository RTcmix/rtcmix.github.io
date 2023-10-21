---
title: HOLO()
layout: ref
---

## HOLO

Stereo FIR filter to perform crosstalk cancellation.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**HOLO**(outsk, insk, dur, AMP, XTALKMULT)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = crosstalk amplitude multipler

   p3 (amplitude) and p4 (crosstalk amplitude multipler) can receive dynamic
   updates from a table or real-time control source.

   Author Doug Scott
```

  

-----

  
**HOLO** is a simulation of the fun Carver Sonic Hologram generator
(yeah, we grew up in the late 70's...). It does some monkey-business
with phase-cancellation between the two outputs of a stereo signal.

### Usage Notes

Try it out to see how it affects the stereo 'image'. It can be used to
create a very (deceptively) wide stereo field. Psychoacoustics in
action\!

**HOLO** only operates on stereo input files and only writes stereo
output files.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("HOLO")

   rtinput("mysoundfile.aiff")

   HOLO(0, 0, 7.8, 0.7, 0.5)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [MIX](MIX.html),
[STEREO](STEREO.html)
