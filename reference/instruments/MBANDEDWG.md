---
title: MBANDEDWG()
layout: ref
---

## MBANDEDWG

Banded-waveguide physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MBANDEDWG**(outsk, dur, AMP, FREQ, strikepos, pluckflag, maxvel,
preset, BOWPRESSURE, RESONANCE, INTEGRATION\[, PAN, VELOCITYENV\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3 = frequency (Hz)
   p4 = strike position (0.0-1.0)
   p5 = pluck flag (0: no pluck, 1: pluck)
   p6 = max velocity (0.0-1.0)
   p7 = preset #
         - Uniform Bar = 0
         - Tuned Bar = 1
         - Glass Harmonica = 2
         - Tibetan Bowl = 3
   p8 = bow pressure (0.0-1.0) 0.0 == strike only
   p9 = mode resonance (0.0-1.0) 0.99 == normal strike
   p10 = integration constant (0.0-1.0) 0.0 == normal?
   p11 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0.5]
   p12 = velocity envelope table reference [optional; default is 1.0]]

   p2 (amplitude), p3 (frequency), p8 (bow pressure), p9 (mode resonance),
   p10 (integration constant) and p11 (pan) can receive dynamic updates from
   a table or real-time control source.

   p11 (velocity table), if used, should be a reference to a pfield table-handle.

   Author:  Brad Garton, based on code from the Synthesis ToolKit
```

  

-----

  
**MBANDEDWG** is the "BandedWG" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the
Synthesis ToolKit.

### Usage Notes

**MBANDEDWG** is a physical model instrument that recreates the sounds
of struck and bowed modal/metal objects (xylophones, glass harmonicas,
Tibetan bowls, etc.).

The description that Perry Cook (and Georg Essl on this one) give about
the "BandedWG" physical model:

Parameter p5 ("pluckflag") is used to determine if the model will be
activated by an impulse. p4 ("strikepos") is used to set how the impulse
will activate the model). Setting p5 to 0 allows the 'bow pressure' (p8,
"BOWPRESSURE"), the 'maximum velocity' (p6, "maxvel") and the 'velocity
envelope' (p12, "VELOCITYENV") to operate. By modulating p12, the sound
of a bowed metal bar can be produced.

The preset selected using p7 ("preset") configures the model for
different sounds. p9 ("RESONANCE") and p10 ("INTEGRATION") also alter
characteristics of the model.

**MBANDEDWG** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MBANDEDWG")

   amp = 20000
   ampenv = maketable("line", 1000, 0,1, 2,0)

   MBANDEDWG(0, 5.0, amp*ampenv, cpspch(8.04), 0.3, 1, 0.5, 3, 0.0, 1.0, 0.0)

   velocityenv = maketable("line", 1000, 0,1, 2,0)
   MBANDEDWG(6, 5.0, amp*ampenv, cpspch(8.04), 0.3, 0, 0.5, 3, 0.3, 0.2, 0.8, 0.5, velocityenv)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MBANDEDWG")

   amp = 10000
   ampenv = maketable("line", 1000, 0,1, 1,0)

   pitches  = { 8.00, 8.02, 8.04, 8.05, 8.07, 8.08, 8.10, 9.00 }
    lpitches = len(pitches)

   st = 0.0
   for (i = 0; i < 50; i = i+1)
   {
      index = trand(0, lpitches)
      pch = pitches[index]
      MBANDEDWG(st, 1.0, amp*ampenv, cpspch(pch), 0.3, 1, 0.5, 0, 0.0, 1.0, 0.0, random())
      st = st + 0.1
   }

   for (i = 0; i < 50; i = i+1)
   {
      index = trand(0, lpitches)
      pch = pitches[index]
      MBANDEDWG(st, 1.0, amp*ampenv, cpspch(pch), 0.3, 1, 0.5, 1, 0.0, 1.0, 0.0, random())
      st = st + 0.15
   }

   for (i = 0; i < 50; i = i+1)
   {
      index = trand(0, lpitches)
      pch = pitches[index]
      MBANDEDWG(st, 1.0, amp*ampenv, cpspch(pch), 0.3, 1, 0.5, 2, 0.0, 1.0, 0.0, random())
      st = st + 0.2
   }

   for (i = 0; i < 50; i = i+1)
   {
      index = trand(0, lpitches)
      pch = pitches[index]
      MBANDEDWG(st, 1.0, amp*ampenv, cpspch(pch), 0.3, 1, 0.5, 3, 0.0, 1.0, 0.0, random())
      st = st + 0.25
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [AMINST](AMINST.html),
[FMINST](FMINST.html), [MBOWED](MBOWED.html), [MMESH2D](MMESH2D.html),
[MMODALBAR](MMODALBAR.html), [MSHAKERS](MSHAKERS.html),
[STRUM](STRUM.html), [STRUM2](STRUM2.html), [STRUMFB](STRUMFB.html),
[WIGGLE](WIGGLE.html)
