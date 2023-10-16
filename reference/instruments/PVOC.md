---
title: PVOC()
layout: ref
---

## PVOC

Phase vocoder.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**PVOC**(outsk, insk, dur, amp, inputchan, fftsize, windowsize,
decimation, interpolation\[, pitchmult, npoles, oscthreshold\])

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of analyzed input signal)
   p4 = input channel
   p5 = fft size (samples, power of 2)
   p6 = window size (samples, normally 2 * fft size)
   p7 = decimation amount (samples, amount to read in [should be < p5])
   p8 = interpolation amount (samples, amount to write out)
   p9 = pitch multiplier [optional; default is 0.0]
   p10 = npoles [optional; default is 0.0]
   p11 = gain threshold for resynthesis [optional; default is 0.0]

   Author:  Doug Scott
```

  

-----

  
**PVOC** (*Phase vocoding*) is an analysis/resynthesis technique whereby
a sound is analyzed through a filter bank with additional computation of
phase deviation from each of the channels of the vocoder (sort of an
expanded Fourier transform). The data discerned from the analysis allows
for realistic time-independent transposition and, by corollary,
pitch-independent time-stretching of a soundfile, with much fewer
resynthesis artifacts than would normally be possible (from Dodge and
Jerse, 1997).

The RTcmix **PVOC** instrument uses a standard FFT analysis with
additional phase computation, allowing the user to specify FFT
parameters and time- and pitch-shifting in terms of multiples of the
original sound.

### Usage Notes

The "fftsize" (p5) determines the resolution in time and frequency of
the anaylsis. This has to be a power of 2. The larger the size (2048,
4096, 8192, etc.) the greater the frequency resolution, but time
resolution suffers -- larger FFT windows 'smear' the signal in time. A
lower "fftsize" parameter (64, 128, 256) resolves time-events, but will
not have as fine a representation of the frequency spectrum. Such is
life.

The "windowsize" parameter (p6) sets how much overlap will occur between
analysis windows (chunks) on output synthesis. The amount of overlap is
p6 compared to p5. Larger values can create a smoother sound, but it may
start sounding a bit reverberant. A value of twice the "fftsize" is
usually reasonable. It should also be a power of 2.

The ratio between p7 ("decimation") and p8 ("interpolation") determines
how much time-dilation or time-compression will occur. The time-scaling
factor is p8/p7. These don't have to be a power of 2, and smaller
numbers may yield smoother results. Smaller values are more
computationally expensive, however. An example of how the time-shifting
works: if p7 is 50 and p8 is 100, then the resynthesized sound will be
twice as long as the original sound. If p7 is set to 300 and p8 is 100,
the resulting sound will be 0.333 times (100/300) as long (three times
faster) than the original sound.

If the optional "pitchmult" (p9) is 0, then **PVOC** will do an
inverse-FFT resynthesis (fairly efficient). If it is \> 0, however, it
will cause an oscillator-bank resynthesis, with inidvidual oscillators
tracking the frequency and amplitude from the FFT analysis. p9 is a
direct multiplier of all frequency values, so that a value of 2.0 will
shift the entire spectrum up one octave, a value of 0.25 will shift it
down two octaves. The following score fragment can be used to calculate
an oct.pc transposition:

``` 
   transposition = 0.05   // shift up 5 semitones
   pitch_multiplier = cpspch(transposition) / cpspch(0.0)
```

If p9 is \> 0, it also allows for the use of LPC-generated amplitude
coefficients for the spectral envelope resynthesis. The optional
"npoles" parameter (p10) can set how many poles the LPC filter will
have. Smaller values create a very approximate spectral resynthesis, and
larger values can generate a filter that is too "lumpy". Usually values
between 20 - 40 are good starting points. A value of 0 will turn off
this feature.

Also if p9 \> 0, the optional "oscthreshold" (p11) parameter is engaged.
During oscillator-bank rsynthesis, only parts of the frequency spectrum
with amplitudes greater than this value will be resynthesized. Values \>
1.0 will generally start having an effect on the output sound. This
feature can be useful for eliminating noise from a signal, although it
will cause audible artifacts in the resulting sound.

Although there is no amplitude envelope control (not even using the
older **makegen** system), it is possible to provide an amplitude
envelope by routing the output of **PVOC** into an instrument that does
have this capability such as [MIX](MIX.html) or [STEREO](STEREO.html).
You can do this using the [bus\_config](../scorefile/bus_config.html)
system.

**PVOC** can read mono or stereo input files; it only writes mono
output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1, 512)
   load("PVOC")

   rtinput("mysound.aif")

   PVOC(start=0, inputskip=0, inputread=DUR(0), amp=0.9, inputchan=0, fft=1024, window=2*fft,
      readin=1024, writeout=2*readin)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 1)
   load("PVOC")

   rtinput("mysound.aif")

   // Resynthesize with oscillator bank, at 0.5 the orig pitch,
   // and only with oscillators > 1.1 in amplitude
   PVOC(0, 0, DUR(0), 1, 0, 1024, 2048, 100, 100, 0.5, 0, 1.1)
```

  
  
fun stuff\!

``` 
   rtsetparams(44100, 1)
   load("PVOC")

   rtinput("mysound.aif")

   start = 0
   inskip = 0
   duration = DUR(0)
   gain = 1
   inskip = 0
   fftsize = 2048
   winsize = 2048*2
   pitch = 1
   decim = 512
   interp = 512
   
   PVOC(start, inskip, duration, gain, 0, fftsize, winsize, decim, interp, pitch)
   
   start = start + duration
   pitch = pitch * 0.8
   PVOC(start, inskip, duration, gain, 0, fftsize, winsize, decim, interp, pitch)
   
   start = start + duration
   pitch = pitch * 0.8
   PVOC(start, inskip, duration, gain, 0, fftsize, winsize, decim, interp, pitch)
   
   start = start + duration
   pitch = pitch * 0.8
   PVOC(start, inskip, duration, gain, 0, fftsize, winsize, decim, interp, pitch)
```

  

-----

### See Also

[CONVOLVE1](CONVOLVE1.html), [LPCPLAY](LPCPLAY.html),
[SPECTACLE](SPECTACLE.html), [SPECTACLE2](SPECTACLE2.html),
[SPECTEQ](SPECTEQ.html), [SPECTEQ2](SPECTEQ2.html),
[TVSPECTACLE](TVSPECTACLE.html), [VOCODE2](VOCODE2.html),
[VOCODE3](VOCODE3.html), [VOCODESYNTH](VOCODESYNTH.html)
