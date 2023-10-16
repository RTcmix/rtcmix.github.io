---
title: EQ()
layout: ref
---

## EQ

Equalizer instrument (peak/notch, shelving and high/low pass filter).

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**EQ**(outsk, insk, dur, AMP, EQTYPE, inputchan, PAN, BYPASS, FILTFREQ,
FILTQ\[, FILTAMP\])

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
   p4 = EQ type ("lowpass", "highpass", "lowshelf", "highshelf", "peaknotch";
      or numeric codes for the EQ type (0: lowpass, 1: highpass, 2: lowshelf, 3: highshelf, 4: peaknotch)
   p5 = input channel
   p6 = pan (0-1 stereo; 0.5 is middle)
   p7 = bypass filter (0: bypass off, 1: bypass on) (usually use 0)
   p8 = filter frequency (Hz)
   p9 = filter Q (values from 0.5 to 10.0, roughly)
   p10 = filter gain (dB) [optional; shelf and peak/notch only]
         use gen 4] *****

   p3 (amplitude), p4 (type), p6 (pan), p7 (bypass), p8 (freq), p9 (Q)
   and p10 (gain) can receive dynamic updates from a table or real-time
   control source.  p4 (type) can be updated only when using numeric codes.

   Author: John Gibson, 7 Dec 2003; rev for v4, 7/23/04
   Based on formulas by Robert Bristow-Johnson ("Audio-EQ-Cookbook") and code
   by Tom St Denis (see musicdsp.org)
```

  

-----

  
**EQ** is an "equalizer" (another name for "filter") instrument able to
instantiate several different types of audio filters.

### Usage Notes

**EQ** can be set to do several different types of filters (peak/notch,
shelving and high/low pass types), depending on the setting of p4
("EQTYPE").

A standard 'biquad' DSP filter algorithm is used to build these
different types of filters.

In addition to the filter frequency and gain, the 'Q' ("FILTQ", p9) can
be used to determine how 'steep' the filter will be. Filter parameters
are interpreted depening on what type of filter (p4) is selected.

The [MULTEQ](MULTEQ.html) instrument allows for the processing of
multiple bands, each (up to 8) a separate instantiation of **EQ**.

The output of **EQ** can be either mono or stereo.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("EQ")
   
   rtinput("mystereofile.wav")
   inskip = 0
   dur = DUR()
   amp = 0.6
   bypass = 0
   type = "lowshelf"
   freq = 500
   Q = 3.0
   gain = 6.0
   
   EQ(0, inskip, dur, amp, type, 0, 1, bypass, freq, Q, gain)
   EQ(0, inskip, dur, amp, type, 1, 0, bypass, freq, Q, gain)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("EQ")
   
   rtinput("mysound.aiff")
   inskip = 0
   dur = DUR()
   
   start = 2
   amp = 1
   bypass = 0
   
   //type = "lowpass"
   type = "highpass"
   //type = "lowshelf"
   //type = "highshelf"
   //type = "peaknotch"
   
   pan = maketable("line", 100, 0,1, 1,0)
   freq = makeconnection("mouse", "x", min=200, max=2000, dflt=min, lag=50,
            "freq", "Hz", 1)
   Q = 1.0
   gain = makeconnection("mouse", "y", min=-30, max=20, dflt=0, lag=50,
            "gain", "dB", 1)
   
   EQ(start, inskip, dur, amp, type, inchan=0, pan, bypass, freq, Q, gain)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconnection](../scorefile/makeconnection.html),
[BUTTER](BUTTER.html), [ELL](ELL.html), [FIR](FIR.html),
[FILTSWEEP](FILTSWEEP.html), [FILTERBANK](FILTERBANK.html),
[FOLLOWBUTTER](FOLLOWBUTTER.html), [IIR](IIR.html), [JFIR](JFIR.html),
[MOOGVCF](MOOGVCF.html), [MULTEQ](MULTEQ.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html)
