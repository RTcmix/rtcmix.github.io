---
title: SPECTEQ2()
layout: ref
---

## SPECTEQ2

FFT-based EQ.

*in RTcmix/insts/jg/SPECTACLE2*  
  

-----

##### quick syntax:

**SPECTEQ2**(outsk, insk, dur, AMP, fftsize, windowsize, WINDOWTABLE,
overlap, EQTABLE\[, MINFREQ, MAXFREQ, BYPASS, inputchan, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0  = output start time (seconds)
   p1  = input start time (seconds)
   p2  = duration (seconds)
   p3  = amplitude multiplier (relative multiplier of input signal)
   p4  = FFT length (samples, power of 2, usually 1024)
   p5  = window length (samples, power of 2, usually FFT length * 2)
   p6  = window table (or zero for internally generated Hamming window)
   p7  = overlap - how much FFT windows overlap (positive power of 2)
      1: no overlap, 2: hopsize=FFTlen/2, 4: hopsize=FFTlen/4, etc.
      2 or 4 is usually fine; 1 is fluttery; higher overlaps use more CPU.
   p8  = EQ table (i.e., amplitude scaling of each band),
      in dB (0 dB means no change, + dB boost, - dB cut).
   p9  = minimum frequency (Hz) [optional; default is 0 Hz]
   p10 = maximum frequency (Hz) [optional; default is Nyquist] 
   p11 = bypass (0: bypass off, 1: bypass on) [optional; default is 0]
   p12 = input channel [optional; default is 0]
   p13 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (amp), p9 (min. freq.), p10 (max. freq.), p11 (bypass) and p13 (pan)
   can receive dynamic updates from a table or real-time control source.

   p6 (window table, if used) and p8 (EQ table) should be
   references to pfield table-handles.

   Author:  John Gibson, 6/12/05
```

  

-----

  
**SPECTEQ2** is an evolution of the earlier [SPECTEQ](SPECTEQ.html)
instrument. It can do very specific filtering jobs, operating directly
on the FFT analysis of a signal spectrum. <span id="usage_notes"></span>

### Usage Notes

**SPECTEQ2** is very similar in design to the
[SPECTACLE2](SPECTACLE2.html) instrument. You may wish to consult the
[SPECTACLE2 Usage Notes](SPECTACLE2.html#usage_notes) for additional
information.

As in [SPECTACLE2](SPECTACLE2.html), it is possible to update the EQ
table (p8, "EQTABLE") dynamically using the [modtable(table, "draw",
...)](../scorefile/modtable.html#draw) scorefile command.

Output begins after a brief period of time during which internal buffers
are filling. This time is the duration corresponding to the following
number of sample frames: window length - (fft length / overlap).

Parameters p9 ("MINFREQ") and p10 ("MAXFREQ") operate in a similar way
to the range parameters in [SPECTACLE2](SPECTACLE2.html).

**SPECTEQ2** can produce either mono or stereo output.  
  
  
There are no sample scorefiles.  

-----

### See Also

[CONVOLVE1](CONVOLVE1.html), [LPCPLAY](LPCPLAY.html), [PVOC](PVOC.html),
[SPECTACLE](SPECTACLE.html), [SPECTACLE2](SPECTACLE2.html),
[SPECTEQ](SPECTEQ.html), [TVSPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html),
[VOCODESYNTH](VOCODESYNTH.html)
