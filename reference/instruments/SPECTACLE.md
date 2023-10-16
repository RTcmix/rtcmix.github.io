---
title: SPECTACLE()
layout: ref
---

## SPECTACLE

FFT-based delay processor.

*in RTcmix/insts/jg/SPECTACLE*  
  

-----

##### quick syntax:

**SPECTACLE**(outsk, insk, dur, amp, ringdowndur, fftsize, windowsize,
windowtype, overlap\[, sigmix, inputchan, pan\])

-----

  
NOTE: This is an older RTcmix instrument, the newer
[SPECTACLE2](SPECTACLE2.html) instrument is probably better to use.  
  
  

``` 
   p0  = output start time (seconds)
   p1  = input start time (seconds)
   p2  = input duration (seconds)
   p3  = amplitude multiplier (relative multiplier of input signal)
   p4  = ring-down duration (seconds)
   p5  = FFT length (samples, power of 2, usually 1024)
   p6  = window length (samples, power of 2, usually FFT length * 2)
   p7  = window type (0: Hamming, 1: Hanning, 2: Rectangle, 3: Triangle, 4: Blackman, 5: Kaiser)
   p8  = overlap - how much FFT windows overlap (samples, any power of 2)
      1: no overlap, 2: hopsize=FFTlen/2, 4: hopsize=FFTlen/4, etc.
      2 or 4 is usually fine; 1 is fluttery; the higher the more CPU time
   p9  = wet/dry mix (0: dry -> 1: wet) [optional; default is 1]
   p10 = input channel [optional; default is 0]
   p11 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]


   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Function table 1 is the input amplitude, spanning just the input duration.
   Function table 2 is the output amplitude, spanning the entire note, including ring-down duration.
   Function table 3 is the EQ table (i.e., amplitude scaling of each band),
      in dB (0 dB means no change, + dB boost, - dB cut).
   Function table 4 is the delay time table.
   Function table 5 is the delay feedback table.  Values > 1 are dangerous!

   Author:  John Gibson
```

  

-----

  
**SPECTACLE** is an FFT-based delay instrument inspired by the totally
awesome Spektral Delay plug-in by Native Instruments (but minus the
interactive ability and some other stuff).

NOTE: This is an older RTcmix instrument, the newer
[SPECTACLE2](SPECTACLE2.html) instrument is probably better to use.
<span id="usage_notes"></span>

### Usage Notes

**SPECTACLE** is not quite like the current [PVOC](PVOC.html), though
obviously related. Basically, each FFT bin gets fitted with its own
recirculating delay line, and the delay times and feedbacks are
controlled by [makegens](../scorefile/makegen.html). For example, if you
randomize the delay times, you get wild repetitive, spectrally
splintered patterns.

Several [makegen](../scorefile/makegen.html) function tables control EQ,
delay time, and delay feedback (tables 3, 4 and 5) for all the frequency
bands of the FFT. Think of them as curves on a graph with frequency on
the x axis and amplitude, delay time or feedback on the y axis.

  - Function table 3 is the EQ table (i.e., amplitude scaling of each
    band), n dB (0 dB means no change, + dB boost, - dB cut).  
      
  - Function table 4 is the delay time table.  
      
  - Function table 5 is the delay feedback table. Values \> 1 are
    dangerous\!

The "windowtype" parameter (p7) allows you to choose the windowing
function used for the FFT analysis. The 'Hamming' window is usually
used.

Parameter p8 ("overlap") can also be negative powers of 2, e.g., .5,
.25, .125, etc. This leaves a gap between successive FFTs, creating ugly
robotic effects -- beware of clipping.

**SPECTACLE** can produce either mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("SPECTACLE")

   /* ftp://presto.music.virginia.edu/pub/rtcmix/snd/ah.snd */
   rtinput("mysound.snd")

   inchan = 0
   inskip = 0
   indur = 2
   ringdur = 15            /* play after indur elapses, while delay lines flush */
   amp = 1
   wetdry = 1              /* 100% wet */

   fftlen = 1024           /* yielding 512 frequency bands */
   winlen = fftlen * 2     /* the standard window length is twice FFT size */
   overlap = 2             /* 2 hops per fftlen (4 per window) */
   wintype = 0             /* use Hamming window */

   /* input envelope (covering ) */
   makegen(1, 18, 1000, 0,0, 1,1, 19,1, 20,0)

   /* output envelope (covering  + ) */
   makegen(2, 4, 1000, 0,1,0, 1,1,-1, 2,0)

   /* EQ curve: -90 dB at 0 Hz, ramping up to 0 dB at 300 Hz, etc. */
   makegen(3, 18, 1000, 0,-90, 100,-90, 300,0, 8000,0, 22050,-90)

   /* fixed delay times between .4 and 3, randomly spread across spectrum */
   min = .4; max = 3
   seed = 1
   makegen(4, 20, 1000, 0, seed, min, max)

   /* constant feedback of 80% for all freq. bands */
   fb = .8
   makegen(5, 18, 1000, 0,fb, 1,fb)

   /* do it for the left chan! */
   start = 0
   SPECTACLE(start, inskip, indur, amp, ringdur, fftlen, winlen, wintype, overlap, wetdry, inchan, pctleft=1)

   /* shift delay table to make right channel sound different */
   shiftgen(4, 500)

   /* do it for the right chan! */
   SPECTACLE(start, inskip, indur, amp, ringdur, fftlen, winlen, wintype, overlap, wetdry, inchan, pctleft=0)
```

  

-----

### See Also

[CONVOLVE1](CONVOLVE1.html), [LPCPLAY](LPCPLAY.html), [PVOC](PVOC.html),
[SPECTACLE2](SPECTACLE2.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html), [TVSPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html),
[VOCODESYNTH](VOCODESYNTH.html)
