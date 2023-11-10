---
title: SPECTEQ()
layout: ref
---

## SPECTEQ

FFT-based EQ.

*in RTcmix/insts/jg/SPECTACLE*  
  

-----

##### quick syntax:

**SPECTEQ**(outsk, insk, dur, amp, ringdowndur, fftsize, windowsize,
windowtype, overlap\[, inputchan, pan\])

-----

  
NOTE: This is an older RTcmix instrument, the newer
[SPECTEQ2](SPECTEQ2.html) instrument is probably better to use.  
  
  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | no | no | 
p4 | ring-down duration | seconds, can be 0 | no | no | 
p5 | FFT length | samples, power of 2 | no | no | usually 1024 |
p6 | window length | samples, power of 2 | no | no | usually FFT length * 2 |
p7 | window type | 0: Hamming, 1: Hanning, 2: Rectangle, 3: Triangle, 4: Blackman, 5: Kaiser | no | no | 
p8 | overlap - how much FFT windows overlap | samples, any power of 2 | no | no | 1: no overlap, 2: hopsize=FFTlen/2, 4: hopsize=FFTlen/4, etc. 2 or 4 is usually fine; 1 is fluttery; the higher the more CPU timr | 
p9 | input channel |  -  | no | yes | default: 0 | 
p10 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 


   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Function table 1 is the input amplitude, spanning just the input duration.
   Function table 2 is the output amplitude, spanning the entire note, including ring-down duration.
   Function table 3 is the EQ table (i.e., amplitude scaling of each band),
      in dB (0 dB means no change, + dB boost, - dB cut).

   Author:  John Gibson

  

-----

  
**SPECTEQ** is an FFT-based EQ/filter, part of the
[SPECTACLE](SPECTACLE.html) family of fft-based fun things.

NOTE: This is an older RTcmix instrument, the newer
[SPECTEQ2](SPECTEQ2.html) instrument is probably better to use.

### Usage Notes

**SPECTEQ** allows you to directly control the amplitude of each band of
an FFT analysis, thus giving you control over the shape of the
resynthesized spectrum. This is a very powerful EQ/filter tool.

The parameters are very similar to the [SPECTACLE](SPECTACLE.html)
instrument, see the [SPECTACLE Usage Notes](SPECTACLE.html#usage_notes)
for more information.

The main thing is to set up function table 3 as the EQ curve. Think of
it as an x-axis map of all the frequency bands of the FFT. The y values
are then the amount of boost or cut for each FFT band (in relative dB --
0 dB means no change, + dB boost, - dB cut).

**SPECTEQ** can produce either mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("SPECTEQ")
   
   rtinput("mysound.snd")
   
   inchan = 0
   inskip = 0
   indur = DUR()
   ringdur = 0
   amp = 5
   fftlen = 1024           /* yielding 512 frequency bands */
   winlen = fftlen * 2     /* the standard window length is twice FFT size */
   overlap = 2             /* 2 hops per fftlen (4 per window) */
   wintype = 0             /* use Hamming window */

   /* input envelope (covering ) */
   makegen(1, 18, 1000, 0,0, 1,1, 19,1, 20,0)

   /* output envelope (covering  + ) */
   copygen(2, 1)

   nyquist = SR()/2
   /* EQ curve: -90 dB at 0 Hz, ramping up to -10 dB at 400 Hz, etc. */
   makegen(3, 18, nyquist/10,
          0,   -90,
        300,   -90,
        400,   -10,
        800,   -20,
       1000,   -90,
       2000,   -90,
       5000,   0,
    nyquist,   -40)

   /* do it for the left chan! */
   start = 0
   SPECTEQ(start, inskip, indur, amp, ringdur, fftlen, winlen, wintype, overlap, inchan, pctleft=1)

   /* do it for the right chan! */
   SPECTEQ(start, inskip, indur, amp, ringdur, fftlen, winlen, wintype, overlap, inchan, pctleft=0)
```

  

-----

### See Also

[CONVOLVE1](CONVOLVE1.html), [LPCPLAY](LPCPLAY.html), [PVOC](PVOC.html),
[SPECTACLE](SPECTACLE.html), [SPECTACLE2](SPECTACLE2.html),
[SPECTEQ2](SPECTEQ2.html), [TVSPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html),
[VOCODESYNTH](VOCODESYNTH.html)
