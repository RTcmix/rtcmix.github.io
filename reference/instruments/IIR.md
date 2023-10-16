---
title: IIR()
layout: ref
---

## IIR

Infinite impulse response filter.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**setup**(centfreq1, bandwidth1, amp1, centfreq2, bandwidth2, amp2,
...)  
  
**INPUTSIG**(outsk, insk, dur, AMP\[, inputchan, PAN\])  
  
**IINOISE**(outsk, dur, AMP\[, PAN\])  
  
**BUZZ**(outsk, dur, AMP, PITCH (Hz/oct.pc)\[, PAN\])  
  
**PULSE**(outsk, dur, AMP, PITCH (Hz/oct.pc)\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**IIR** consists of a set of sub-instruments that draw upon a subcommand
(**setup**) for filter design parameters.  
  
  
<span id="setup"></span> **setup**  

``` 
   The pfields for setup are triples, the first being the center frequency
   of an IIR filter "hump" (in Hz), the second being the bandwidth of the
   "hump" (Hz, or if negative a multiplier of the center frequency), and
   the third being the relative amplitude of that "hump" in the final
   constructed filter.  Up to 64 cf-bw-amp triples can be specified.
```

  
<span id="INPUTSIG"></span> **INPUTSIG**  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = input channel [optional, default is 0]
   p5 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (amplitude) and p5 (pan) can receive dynamic updates from a table
   or real-time control source.
```

  
<span id="IINOISE"></span> **IINOISE**  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amp (absolute, for 16-bit soundfiles: 0-32768)
   p3 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p2 (amplitude) and p3 (pan) can receive dynamic updates from a table
   or real-time control source.
```

  
<span id="BUZZ"></span> **BUZZ**  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amp (absolute, for 16-bit soundfiles: 0-32768)
   p3 = pitch (Hz or oct.pc *) (see note below)
   p4 = pan (in percent-to-left form: 0-1) [optional, default is 0] 

   p2 (amplitude), p3 (pitch) and p4 (pan) can receive dynamic updates
   from a table or real-time control source.

   * If the value of p3 field is < 15.0, it assumes oct.pc.  Use the pchcps
   scorefile convertor for direct frequency specification below 15.0 Hz.
```

  
<span id="PULSE"></span> **PULSE**  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amp (absolute, for 16-bit soundfiles: 0-32768)
   p3 = pitch (Hz or oct.pc *) (see note below)
   p4 = pan (in percent-to-left form: 0-1) [optional, default is 0]

   p2 (amplitude), p3 (pitch) and p4 (pan) can receive dynamic updates
   from a table or real-time control source.

   * If the value of p3 field is < 15.0, it assumes oct.pc.  Use the pchcps
   scorefile convertor for direct frequency specification below 15.0 Hz.

   rev. for v4.0 of all the above by JGG, 7/10/04
```

  

-----

  
**IIR** sets up an infinite impulse response (or recursive) filter with
center frequency, bandwidth, and amplitude boost defined in triplets by
the **setup** subcommand. **IIR** can take as its input a soundfile
(**INPUTSIG**), white noise (**IINOISE**), a pulse train (**PULSE**), or
a buzzing signal (**BUZZ**). **IIR** filters are excited based on
previous output samples as well as previous input samples, so that they
can ring down infinitely using the equation:

where *y* is an output sample at time *n*, *x* is an input sample, and
*a<sub>0</sub>* and *b<sub>N</sub>* are filter coefficients that are
determined by the shape of the filter desired (blatantly stolen from
Dodge and Jerse, 1985). The number of coefficients used based on past
output samples is called the number of *poles* in the filter. **IIR**
uses a standard 4-pole filter equation. At least, that's what we'd like
you to think.

### Usage Notes

**IIR** is an older instrument, reflected in the 'subcommand' structure
of the setup and instrument calls in the score. It has largely been
superceded by the [FILTERBANK](FILTERBANK.html) instrument which allows
more control (pfield-enabled) flexibility. However, **FILTERBANK** does
not have the **BUZZ** and **PULSE** capabilities. Both are useful for
simulating speech-like sounds for formant processing in **IIR**.

**BUZZ** generates a waveform consisting of all harmonic partials at
relative amplitude 1.0 between the specified pitch and the Nyquist
frequency. **PULSE** generates a unit impulse at the specified frequency
and pitch. Although they are similar, there are differences in the sound
related to the band-limited nature of **BUZZ** as opposed to **PULSE**.

For the pitch specification, oct.pc format generally will not work as
you expect for p3 (osc freq) if the pfield changes dynamically because
of the 'mod 12' aspect of the pitch-class (.pc) specification. Use
direct frequency (hz) or linear octaves instead.

The **IIR** instruments can produce either stereo or mono output.

NOTE: Older versions of the **IIR** family of commands used **NOISE**
instead of **IINOISE**. Unfortunately this conflicted with the generic
RTcmix instrument [NOISE](NOISE.html), so the name was changed.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1)
   load("IIR")

   rtinput("mysoundfile.aif")

   ampenv = maketable("line", 1000, 0,0, 1,1, 5,1, 7,0)
   setup(149.0, 25.0, 1.0, 1415.0, 100.0, 0.8)
   INPUTSIG(0, 0, 7, 0.25*ampenv, 0)

   setup(90.0, -0.5, 1.0, 1000.0, -0.1, 0.8)
   INPUTSIG(8, 0, 7, 0.15, 0)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("IIR")

   ampenv = maketable("line", 1000, 0,0, 0.2,1, 0.3,0)

   start = 0
   for(pc = 0; pc < 0.25; pc = pc + 0.01) {
      setup(8.00 + pc, 1.0, 1.0)
      IINOISE(start, 0.3, 15000*ampenv, random())
      start = start + 0.1
   }
```

  
  
fun stuff\!

``` 
   rtsetparams(44100, 2)
   load("IIR")

   env = maketable("window", 1000, "hanning")

   pitch = 134.0
   for(start = 0; start < 7.8; start = start + 0.1) {
      setup((random()*2000.0) + 300.0, -0.5, 1)

      BUZZ(start, 0.1, 40000*env, pitch, random())
      BUZZ(start, 0.1, 40000*env, pitch + 2.5, random())
      pitch = pitch + 0.5
   }

   for(start = 7.8; start < 15; start = start + 0.1) {
      setup((random()*2000.0) + 200.0, -0.5, 1)

      PULSE(start, 0.1, 40000*env, pitch, random())
      PULSE(start, 0.1, 40000*env, pitch + 2.5, random())
      pitch = pitch - 0.5
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [BUTTER](BUTTER.html),
[ELL](ELL.html), [EQ](EQ.html), [FILTERBANK](FILTERBANK.html),
[FILTSWEEP](FILTSWEEP.html), [FIR](FIR.html), [JFIR](JFIR.html),
[MOOGVCF](MOOGVCF.html), [MULTEQ](MULTEQ.html)
