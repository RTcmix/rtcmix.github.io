---
title: SHAPE()
layout: ref
---

## SHAPE

Input sound waveshaping instrument.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**SHAPE**(outsk, insk, dur, AMP, MINDIST, MAXDIST, AMPNORMTABLE,
inputchan, PAN, TRANSFERFUNC\[, INDEXENV\])

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
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = minimum distortion index (0.0-1.0)
   p5 = maximum distortion index (0.0-1.0)
   p6 = reference to an amplitude normalization table, or 0 for no normalization
   p7 = input channel
   p8 = pan (0-1 stereo; 0.5 is middle)
   p9 = reference to waveshaping tranfer function table
   p10 = index control envelope [optional; default is constant 1.0]

   p3 (amplitude), p4 (min index), p5 (max index), p8 (pan) and p10 (index)
   can receive dynamic updates from a table or real-time control source.

   p6 (amp normalization table) and p9 (transfer function) should be references to pfield table-handles.

   Author:  John Gibson, 3 Jan 2002; rev for v4, 7/21/04
```

  

-----

  
**SHAPE** does a table-lookup waveshaping (distortion) on an arbitrary
input soundfile. The [WAVESHAPE](WAVESHAPE.html) instrument uses a
similar approach, but does synthesis instead of signal-processing.

### Usage Notes

**SHAPE** accepts input from a file, a real-time audio device, or from a
bus. It also offers a different way of performing amplitude
normalization than [WAVESHAPE](WAVESHAPE.html). **SHAPE** accomplishes
this by applying a waveshaping transfer function to the input source.
(See Dodge and Jerse, or any other computer music text, for a detailed
explanation of this technique.)

For a short explanation of the waveshaping process and how to configure
the transfer function (p8, "TRANSFERFUNCTION") along with the index
control function (p9, "INDEXENV" if used) and index constraints (p3 and
p4, "INDEXMIN", "INDEXMAX"), see the [WAVESHAPE Usage
Notes](WAVESHAPE.html#usage_notes).

If you want to perform amplitude normalization, use a pfield table
through p6 ("AMPNORMTABLE") with the normalization curve. Otherwise, set
p6 to zero. The amplitude normalization control table allows you to
decouple amplitude and timbral change, to some extent. (Read about this
in the Roads "Computer Music Tutorial'' chapter on waveshaping.) The
table maps incoming signal amplitudes, on the X axis, to amplitude
multipliers, on the Y axis. The multipliers should be from 0 to 1. The
amplitude multipliers are applied to the output signal, along with
whatever overall amplitude curve is in effect. Generally, you'll want a
curve that starts high and moves lower for the higher signal amplitudes.
This will keep the bright and dark timbres more equal in amplitude. But
anything is possible.

The output of **SHAPE** can be either stereo or mono.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("WAVETABLE")
   load("SHAPE")

   bus_config("WAVETABLE", "aux 0 out")
   bus_config("SHAPE", "aux 0 in", "out 0-1")

   dur = 10
   amp = 10000
   freq = 200
   start = 0
   WAVETABLE(start, dur, amp, freq, 0)

   amp = 1.0
   ampenv = maketable("line", 1000, 0,0, 9,1, 10,0)

   transferfunc = maketable("cheby", "nonorm", 1000, 0.9, 0.3, -0.2, 0.6, -0.7, 0.9, -0.1)

   min = 0; max = 3.0
   indexguide = maketable("window", 1000, "hanning")    // bell curve

   SHAPE(start, inskip = 0, dur, amp*ampenv, min, max, 0, inchan=0, pan=0.5, transferfunc, indexguide)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("WAVETABLE")
   load("SHAPE")

   bus_config("WAVETABLE", "aux 0 out")
   bus_config("SHAPE", "aux 0 in", "out 0-1")

   dur = 20
   freq = 60

   amp = 20000
   wavet = maketable("wave", 2000, 1, .3, .1)
   WAVETABLE(start=0, dur, amp, freq, 0, wavet)

   transferfunc = maketable("wave", 1000, "square13")

   // sample-and-hold distortion index
   speed = 7   // sample-and-hold changes per second
   indexguide = makerandom("even", speed, min=0, max=1, seed=1)
   indexguide = makefilter(indexguide, "smooth", lag=20)
   minidx = 0.0
   maxidx = 3.0

   normfunc = maketable("curve", "nonorm", 1000, 0,1,-1, 1,.3)

   amp = 0.8
   env = maketable("line", 1000, 0,1, dur-.03,1, dur,0)

   SHAPE(start, inskip=0, dur, amp * env, minidx, maxidx, normfunc, 0, 1, transferfunc, indexguide)

   // vary distortion index for other channel
   indexguide = makerandom("even", speed, min=0, max=1, seed + 1)
   indexguide = makefilter(indexguide, "smooth", lag=20)
   SHAPE(start, inskip=0, dur, amp * env, minidx, maxidx, normfunc, 0, 0, transferfunc, indexguide)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makerandom](../scorefile/makerandom.html),
[makefilter](../scorefile/makefilter.html), [DISTORT](DISTORT.html),
[SHAPE](SHAPE.html), [DECIMATE](DECIMATE.html),
[COMPLIMIT](COMPLIMIT.html)
