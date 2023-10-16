---
title: MULTIFM()
layout: ref
---

## MULTIFM

Configurable multi-oscillator FM synthesis instrument.

*in RTcmix/insts/neil*  
  

-----

##### quick syntax:

**MULTIFM**(outsk, dur, AMP, numosc, PAN, FREQ1, wavet1, ... FREQN,
wavetN, osc1, out1, index1, ... oscN, outN, indexN)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time
   p1 = duration
   p2 = overall amplitude multiplier
   p3 = number of oscillators
   p4 = pan (in percent-to-left format)
   p5, p6 ... pN-1, pN 
      List of frequency / waveform pairs for each oscillator.
      pO, pO+1, pO+2 ... pP-2, pP-1, pP
      Oscillator connections in triples in the following form:

      1) Oscillator (Carrier) - Audio Out - Relative Amplitude
         or
      2) Modulator - Carrier - Index

      The first number refers to an oscillator, 1-N.
         1 refers to the first frequency / waveform pair, p5-p6.
         2 refers to the next pair, and so on.

      The second number directs the output of the indicated oscillator.
         0 directs it to audio output (form 1 above).
         1-N directs it to modulate the frequency of the given oscillator
        (form 2 above).

      The third number is the relative amplitude (for form 1) or the
      index (for form 2).

      Any number of connections in any direction, including feedback, is
      acceptable.

   p2 amplitude, p4 pan, oscillator frequencies, and oscillator amplitude/index
   can receive dynamic updates from a table or real-time control source.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16
```

  

-----

  
**MULTIFM** is a complex (complicated) instrument that allows for the
type of frequency (actually, phase) modulation made famous by the Yamaha
DX7 and other such synthesizers. With this instrument, you can create a
wildly complex FM instrument with any number of oscillators and any
types of connections between them.

**MULTIFM** can produce mono or stereo output.

### Usage Notes

If frequency modulation is new to you, it may be helpful to read up on
it first and get familiar with basic terminology (carrier, modulator,
index, etc.). See, for example, the documentation on
**[FMINST](FMINST.html)**.

p3 is the total number of oscillators used for this instrument. We can
choose, say, to use three oscillators, Oscillators 1, 2, and 3.
Beginning with p5, we will give a frequency/wavetable pair to each
oscillator. p5 is the frequency for Oscillator 1, and p6 is its
wavetable. p7 is the frequency for Oscillator 2, and p8 is its
wavetable. p9 and p10 refer to Oscillator 3.

After the frequency/wavetable pairs, we specify their connections in
sets of three numbers. For example, we can indicate connections as
follows:

``` 
   1, 0, 1,
   2, 0, 0.5,
   2, 1, 5,
   3, 2, 4
```

Each triple specifies 1) which oscillator we are dealing with, 2) where
to connect its output, and 3) its amplitude or index. Output can be 0
(referring to audio out) or another oscillator. In this case, Oscillator
1 sends its signal to audio out at a relative amplitude of 1. Oscillator
2 sends its signal to audio out also, but at half the amplitude of
Oscillator 1. In the third line, Oscillator 2 modulates Oscillator 1
with an index of 5. In the fourth line, Oscillator 3 modulates
Oscillator 2 with an index of 4.

Any kinds of connections are possible. Using five oscillators, we could
specify connections as follows:

``` 
   1, 0, 1,
   2, 1, 3,
   3, 2, 4,
   3, 3, 1,
   4, 2, 5,
   5, 4, 3,
   5, 3, 3,
   2, 5, 1
```

Notice that an oscillator can feed back into itself or can feed back
into an oscillator upstream.

### Sample Score

``` 
   rtsetparams(44100, 2)
   load("MULTIFM")

   freq = 400
   wavet = maketable("wave", 1000, "sine")
   MULTIFM(0, 5, 10000, numosc=3, pan=0.5,
      freq, wavet,
      freq*2.01, wavet,
      freq*2.99, wavet,
      1, 0, 1,
      2, 1, 3,
      3, 2, 2
   )
```

  

-----

### See Also

[AMINST](AMINST.html), [FMINST](FMINST.html),
[MULTIWAVE](MULTIWAVE.html), [WAVETABLE](WAVETABLE.html),
[WAVESHAPE](WAVESHAPE.html), [WAVY](WAVY.html), [WIGGLE](WIGGLE.html)
