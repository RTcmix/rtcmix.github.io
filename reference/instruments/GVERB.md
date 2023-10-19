---
title: GVERB()
layout: ref
---

## GVERB

Very long and smooth reverb algorithm.

*in RTcmix/insts/bgg*  
  

-----

##### quick syntax:

**GVERB**(outsk, insk, dur, AMP, ROOMSIZE, RVBTIME, DAMPING, BANDWIDTH,
DRYLEVEL, EARLYREFLECT, RVBTAIL, RINGDOWN\[, INCHAN\])

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
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = roomsize (1.0 - 300.0)
   p5 = reverb time (0.1 - 360.0)
   p6 = damping (0.0 - 1.0)
   p7 = input filter bandwidth (0.0 - 1.0)
   p8 = dry level (inverse dB, -90.0 - 0.0)
   p9 = early reflection level (inverse dB, -90.0 - 0.0)
   p10 = tail level (inverse dB, -90.0 - 0.0)
   p11 = ring-down time (seconds, added to duration)
   p12 = input channel [optional; default = 0]


   p3 (amplitude), p4 (roomsize), p5 (reverb time), p6 (damping), p7 (input filter bandwidth
   p8 (dry level), p9 (early reflection level) and p10 (tail level) can receive dynamic updates
   from a table or real-time control source.

   Author:  Brad Garton, 5/2010
```

  

-----

  
**GVERB** is a very smooth reverberator with the ability to produce very
long reverb times.

### Usage Notes

**GVERB** is based on the original "gverb/gigaverb" by Juhana Sadeharju
(*kouhia at nic.funet.fi*). The code for this version was taken from the
Max/MSP version by Olaf Mtthes (*olaf.matthes at gmx.de*).

The parameters should be relatively self-explanatory for this
instrument. Note the use of 'inverse' dB to specify the dry signal
amount in the output (p8, "DRYLEVEL"), the early (wall) reflection
leveil (p9, "EARLYREFLECT") and the level of the reverberated signal
'tail' (diffuse reflections, p10, "RVBTAIL").

The filter bandwith parameter (p7, "BANDWIDTH") applies a simple
low-pass filter to the input signal prior to processing through the
reverberator.

The ring-down time (p11, "RINGDOWN") is used to allow the revebr to die
away after the input signal has finished.

If you modulate some of the parameters too quickly using
pfield-controls, some glitching may occur. Use a shorter
[reset](../scorefile/reset.html) value to compensate (may not always
work, though).

**GVERB** requires a stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("GVERB")

   rtinput("mysound.aif")

   GVERB(0, 0, 9.8, 0.9, 78.0, 7.0, 0.71, 0.34, -10.0, -11.0, -9.0, 7.0)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("GVERB")

   rtinput("mysound.aif")

   roomsize = maketable("line", "nonorm", 1000, 0, 78.0, 3,200.0, 7,10.0, 10,25.0)
   revtime = maketable("line", "nonorm", 1000, 0,1, 10,50)
   damping = makeLFO("tri", 0.4, 0.1, 0.9)
   amp = 0.7

   GVERB(0, 0, 9.8, amp, roomsize, revtime, damping, 0.34, -10.0, -11.0, -9.0, 7.0)


   inputbandwidth = makeLFO("tri", 0.5, 0.1, 0.9)
   drylevel = maketable("line", "nonorm", 1000, 0,-1.0,  5,-50.0,  7,-1.0, 15,-1.0)
   amp = 0.3

   GVERB(14, 0, 9.8, amp, 35.0, 15.0, 0.5, inputbandwidth, drylevel, -11.0, -9.0, 5.0)


   earlylevel = maketable("line", "nonorm", 1000, 0,-1.0, 5,-68, 9,-10.0, 15,-10.0)
   taillevel = maketable("line", "nonorm", 1000, 0,-70, 5,-3.5, 10,-50, 15,-50)
   amp = 0.9
   GVERB(25, 0, 9.8, amp, 143.0, 9.0, 0.7, 0.7, -27.0, earlylevel, taillevel, 3.0)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [MMOVE](MMOVE.html),
[MOVE](MOVE.html), [MPLACE](MPLACE.html), [MROOM](MROOM.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
