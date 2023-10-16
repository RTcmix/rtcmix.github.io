---
title: MOOGVCF()
layout: ref
---

## MOOGVCF

24db/octave resonant lowpass filter.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**MOOGVCF**(outsk, insk, dur, AMP, inputchan, PAN, BYPASS,
FILTFREQTABLE, FILTRESONTABLE)

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
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = input channel
   p5 = pan (0-1 stereo; 0.5 is middle)
   p6 = bypass filter (0: bypass off, 1: bypass on) (usually use 0)
   p7 = filter cutoff frequency (Hz)
   p8 = filter resonance (0-1, 1 is more resonant.  > 1.0 will self-oscillate)

   p3 (amplitude), p5 (pan), p6 (bypass), p7 (cutoff) and p8 (resonance) can
   receive dynamic updates from a table or real-time control source.

   Author:  John Gibson, 22 May 2002; rev for v4, 7/24/04
   This is based on the design by Stilson and Smith (CCRMA), as modified
   by Paul Kellett  (described in the source code archives at musicdsp.org).
```

  

-----

  
**MOOGVCF** duplicates the famous "Moog" lowpass filter.

### Usage Notes

One of the characteristics of this filter is a sharp resonant peak at
the lowpass cutoff frequency. The sharpness is determined by the
"FILTRESONTABLE" pfield (p8). Values \> 1.0 will cause self-oscillation
of the filter, producing a tone at the lowpass cutoff frequency.

The setting of the amplitude multiplier (p3, "AMP") can be a little
tricky. If the filter saturates (clips), it will 'explode' (i.e. produce
an infinite output -- the result is no sound).

**MOOGVCF** can produce either stereo or mono output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("WAVETABLE")
   load("MOOGVCF")

   // feed wavetable into filter
   bus_config("WAVETABLE", "aux 0 out")
   bus_config("MOOGVCF", "aux 0 in", "out 0-1")

   dur = 10.0
   amp = 10000
   pitch = 6.00
   wavet = maketable("wave", 15000, "saw30")
   WAVETABLE(0, dur, amp, pitch, 0, wavet)
   WAVETABLE(0, dur, amp, pitch+.0005, 0, wavet)

   amp = 20.0
   env = maketable("line", 1000, 0,1, 7,1, 10,0)

   lowcf = 500
   highcf = 1500
   lowres = 0.1
   highres = 0.9

   cf = maketable("line", "nonorm", 2000, 0,lowcf, dur*.2,lowcf, dur*.5,highcf, dur,lowcf)
   res = maketable("line", "nonorm", 2000, 0,lowres, 1,highres, 2,lowres)

   MOOGVCF(0, 0, dur, amp * env, inchan=0, pan=0.5, bypass=0, cf, res)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [BUTTER](BUTTER.html),
[ELL](ELL.html), [EQ](EQ.html), [FIR](FIR.html),
[FILTERBANK](FILTERBANK.html), [FILTSWEEP](FILTSWEEP.html),
[FOLLOWBUTTER](FOLLOWBUTTER.html), [IIR](IIR.html), [JFIR](JFIR.html),
[FILTSWEEP](FILTSWEEP.html), [MULTEQ](MULTEQ.html)
[SPECTEQ](SPECTEQ.html), [SPECTEQ2](SPECTEQ2.html)
