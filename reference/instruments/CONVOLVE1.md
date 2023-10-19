---
title: CONVOLVE1()
layout: ref
---

## CONVOLVE1

Real-time convolution.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**CONVOLVE1**(outsk, insk, dur, AMP, IMPULSETABLE, impstart, impudur,
impamp, WINDOWTABLE, AMTWET, inputchan, PAN)

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
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = impulse response table (usually a soundfile, use maketable("soundfile", ...)
      must be mono; maketable lets you select channel.
   p5 = impulse start time (time to skip in table) (seconds)
   p6 = impulse duration (seconds)
   p7 = impulse gain (relative multiplier of signal)
   p8 = window function table
   p9 = wet amount (0-1)
   p10 = input channel
   p11 = pan (0-1 stereo; 0.5 is middle)

   p3 (amplitude), p9 (wet percent) and p11 (pan) can receive dynamic updates
   from a table or real-time control source.

   p4 (impulse response table) and p8 (window function table) should be
   references to pfield table-handles.

   Author: John Gibson, 5/31/05 (based on cmix convolve)
```

  

-----

  
**CONVOLE1** implements the DSP procedure known as *convolution*.
Convolution is the process in which the spectrum of one file (usually
called the "impulse response" file) is multiplied by the spectrum of
another (the "input" file). This can produce a wide variety of effects,
ranging from a good room simulation (if the "impulse response" is a
short recording of a room excited by a true "impulse" sound) to a
vocoding-like effect where one sound source seems to be 'speaking'
through another.

### Usage Notes

This instrument is designed for short impulse responses: a maximum of
about 6 seconds at 44.1kHz sampling rate, though a few hundred
milliseconds is more typical. The impulse response duration determines
the FFT size: the longer the response, the larger the FFT size, and
therefore the more transient smearing. Only one FFT of the impulse
response is taken. The FFT size of the input equals that of the impulse
response, and there is a delay before the start of sound approximately
equal to the impulse response duration. (Actually, the delay in frames
is the smallest power of two that is greater than the impulse response
length.)

With this in mind, the best strategy for using the instrument is to loop
with very short notes, while creeping through both the input and the
impulse response. See the example scores.

The impulse response is read from a table (p4). As noted above, use
[maketable("soundfile", ...)](../scorefile/maketable.html#soundfile) to
read a soundfile into this table.

The "window function table" (p8) determines the window function used by
the FFT. [maketable("window", ...)](../scorefile/maketable.html#window)
is a convenient way to set this up.

p9 ("wet amount") determines how much of the convolved signal will be
mixed in with the original, unprocessed signal.

NOTE: This instrument can work with very small buffer sizes (e.g., 32),
but it probably will cause audio buffer underruns if the impulse
response is longer than about 20 milliseconds.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("CONVOLVE1")
   
   rtinput("mysound.aif")

   inchan = 0
   inskip = 0.0
   indur = DUR()
   
   // random impulse response
   impdur = 0.3
   imptab = maketable("random", "nonorm", impdur * 44100, "even",
      min = -10000, max = 10000, seed = 1)
   impskip = 0.0
   impgain = 4.5
   
   amp = maketable("line", 2000, 0,0, .1,1, indur-.1,1, indur,0)
   wintab = 0
   wetpct = 1.0
   
   CONVOLVE1(0, inskip, indur, 0.7*amp, imptab, impskip, impdur, impgain,
      wintab, wetpct, inchan, pan=1)
   
   // decorrelate channels by shifting noise samples by half table length
   // could also create a new table with a different seed
   imptab = copytable(modtable(imptab, "shift", tablelen(imptab) / 2))
   
   CONVOLVE1(0, inskip, indur, 0.7*amp, imptab, impskip, impdur, impgain,
      wintab, wetpct, inchan, pan=0)
```

  
  
slightly more advanced:

``` 
   // This is an example of walking slowly through both input and impulse
   // response files, convolving a little bit at a time.    -JG
   rtsetparams(44100, 2)
   load("CONVOLVE1")
   load("PANECHO")

   bus_config("CONVOLVE1", "in 0", "aux 0 out")
   bus_config("PANECHO", "aux 0 in", "out 0-1")
   
   rtinput("mysound.aif")
   imp_tab = maketable("soundfile", "nonorm", 0, "myimpulsesound.aif")
   
   outdur = 14
   
   // out_increment affects the "grain" of the output file. It's the skip
   // between successive convolve notes. A smaller number will be more
   // smeary and take longer.
   out_increment = 0.02
   
   // src_dur is the amount of the source file to process at a time.
   // src_inskip is where to start reading.
   // src_increment is how much to increase inskip between convolve notes.
   // src_env is the envelope applied to the segment of the source file.
   src_amp = 2.0
   src_dur = 0.15
   src_inskip = 0
   src_increment = src_dur * 0.02
   src_maxjitter = 0.0
   src_env = maketable("window", 1000, "hanning")
   
   // impulse response file (same as for source file)
   imp_amp = 2.0
   imp_dur = 0.1
   imp_inskip = 0
   imp_increment = imp_dur * 0.01
   imp_maxjitter = 0.0
   
   // window function used for both input and impulse response
   window = maketable("window", 1000, "hanning")
   
   // general parameters
   wet = 1.0
   inchan = 0
   control_rate(20000)
   
   inskip = src_inskip
   impskip = imp_inskip
   for (start = 0; start < outdur; start += out_increment) {
      CONVOLVE1(start, inskip, src_dur, src_amp * src_env,
         imp_tab, impskip, imp_dur, imp_amp, window, wet, inchan, pan=1)
      inskip += src_increment + irand(0, src_maxjitter)
      impskip += imp_increment + irand(0, imp_maxjitter)
   }
   
   env = maketable("line", 1000, 0,0, 4,1, outdur-1,1, outdur,0)
   PANECHO(0, 0, outdur, env, ldel=0.2, rdel=0.77, fb=0.2, ringdur=4.0)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [LPCIN](LPCIN.html),
[PVOC](PVOC.html), [SPECTACLE](SPECTACLE.html),
[SPECTACLE2](SPECTACLE2.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html), [SPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html),
[VOCODESYNTH](VOCODESYNTH.html)
