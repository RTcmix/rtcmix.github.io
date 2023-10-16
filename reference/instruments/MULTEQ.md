---
title: MULTEQ()
layout: ref
---

## MULTEQ

Multi-band equalizer.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**MULTEQ**(outsk, insk, dur, AMP, MASTERBYPASS, EQTYPE1, FILTFREQ1,
FILTQ1, FILTAMP1, FILTBYPASS1, ... EQTYPEN, FILTFREQN, FILTQN, FILTAMPN,
FILTBYPASSN)

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
   p4 = master (bypass 0: bypass off, 1: bypass on) (usually use 0)
   p5, p6, p7, p8, p9 ... pN-4, pN-3, pN-2, pN-1, pN
      starting with p5, the next N pfields are quintuples describing each EQ band:
         - EQ type ("lowpass", "highpass", "lowshelf", "highshelf", "peaknotch")
            or numeric codes for the filter type
            (1: lowpass, 2: highpass, 3: lowshelf, 4: highshelf, 5: peaknotch)
         - filter frequency (Hz)
         - filter Q (c. 0.5-10)
         - filter gain (cut or boost, in dB -- for shelf and peak/notch only)
         - bypass (bypass 0: bypass off, 1: bypass on) (usually use 0)
      Not all have to specified; a maximum of 8 quintuples is allowed.

   p3 (amplitude), p4 (bypass) as well as the EQ type, freq, Q, gain and bypass pfields
   for individual bands can receive dynamic updates from a table or real-time
   control source.  The EQ type pfields can be updated only when using numeric codes

   Author:  John Gibson, 26 Sep 2004
   Based on formulas by Robert Bristow-Johnson ("Audio-EQ-Cookbook") and code
   by Tom St Denis (see musicdsp.org)
```

  

-----

  
  
**MULTEQ** is a very flexible approach to signal filtering, especially
when the pfield capabilities of each individual filter section is
employed. The code is based on the [EQ](EQ.html) instrument.

### Usage Notes

The individual bands of **MULTEQ** can be set to do several different
types of filters (peak/notch, shelving and high/low pass types),
depending on the setting of "EQTYPE" pfields for each one.

A standard 'biquad' DSP filter algorithm is used to build these
different types of filters.

In addition to the filter frequency and gain, the 'Q' ("FILTQ") can be
used to determine how 'steep' each of the filter bands will be. Filter
parameters for the individual bands are interpreted depening on what
type of filter type is selected.

The "FILTAMP" parameter for each filter band is in dB, expressed as a
boost (+dB) or a cut (-dB) that will deviated from the overall master
ampilitude multiplier of the signal ("AMP", p3).

**MULTEQ** is either mono-to-mono or stereo-to-stereo.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("MULTEQ")

   rtinput("mystereofile.wav")

   inskip = 0
   dur = DUR()
   amp = 0.5
   bypass = 0

   type1 = "lowshelf"
   freq1 = maketable("line", "nonorm", 100, 0,100, 1,100, 3,1000)
   Q1 = 10
   gain1 = maketable("line", "nonorm", 100, 0,6, 1,6, 3,-6)
   bypass1 = 0

   type2 = "highshelf"
   freq2 = 2000
   Q2 = 5
   gain2 = maketable("line", "nonorm", 100, 0,-12, 1,9, 2,0)
   bypass2 = 0

   MULTEQ(0, inskip, dur, amp, bypass, type1, freq1, Q1, gain1, bypass1, type2, freq2, Q2, gain2, bypass2)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [BUTTER](BUTTER.html),
[ELL](ELL.html), [FIR](FIR.html), [FILTSWEEP](FILTSWEEP.html),
[FILTERBANK](FILTERBANK.html), [FOLLOWBUTTER](FOLLOWBUTTER.html),
[IIR](IIR.html), [JFIR](JFIR.html), [MOOGVCF](MOOGVCF.html),
[EQ](EQ.html), [SPECTEQ](SPECTEQ.html), [SPECTEQ2](SPECTEQ2.html)
