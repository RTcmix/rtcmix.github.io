---
title: QPAN()
layout: ref
---

## QPAN

4-channel panning of input signal.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**QPAN**(outsk, insk, dur, AMP, XLOC, YLOC\[, inputchan\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = X coordinate of virtual source (-1.0 - 1.0) [-1: left, 1: right, 0.0: center]
   p5 = Y coordinate of virtual source (-1.0 - 1.0) [-1: back, 1: front, 0.0: center]
   p6 = input channel [optional, default is 0]

   p3 (amplitude), p4 (xloc) and p5 (yloc) can receive dynamic
   updates from a table or real-time control source.

   Author:  John Gibson, 11/18/04
```

  

-----

  
**QPAN** is a simple instrument to do quadraphonic panning.

### Usage Notes

The listener is in the center of the coordinate space, at \[0,0\].
Left-to-right panning (p4, "XLOC") is from -1.0 to 1.0; back-to-front
panning (p5 "YLOC") is from -1.0 to 1.0.

The [rtsetparams](../scorefile/rtsetparams.html) scorefile command
should be set for 4 channels, and a sound card capable of sending out 4
independent audio streams is obviously required.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 4)
   load("WAVETABLE")
   load("QPAN")

   bus_config("WAVETABLE", "aux 0 out")
   bus_config("QPAN", "aux 0 in", "out 0-3")

   dur = 60
   amp = 10000
   freq = 440

   wave = maketable("wave", 2000, 1)
   line = maketable("line", 1000, 0,0, 1,1, 19,1, 20,0)

   WAVETABLE(0, dur, amp * line, freq, 0, wave)

   lag = 70
   srcX = makeconnection("mouse", "X", -1, 1, 0, lag, "X")
   srcY = makeconnection("mouse", "Y", -1, 1, 1, lag, "Y")

   QPAN(0, 0, dur, 1, srcX, srcY)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconnection](../scorefile/makeconnection.html), [DMOVE](DMOVE.html),
[MIX](MIX.html), [MMOVE](MMOVE.html), [MOVE](MOVE.html),
[MPLACE](MPLACE.html), [MROOM](MROOM.html), [NPAN](NPAN.html),
[PLACE](PLACE.html), [QPAN](QPAN.html), [ROOM](ROOM.html),
[SROOM](SROOM.html) [STEREO](STEREO.html)
