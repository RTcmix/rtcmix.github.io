---
title: AMINST()
layout: ref
---

## AMINST

Amplitude modulation synthesis.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**AMINST**(outsk, dur, AMP, CARFREQ, MODFREQ\[, PAN, MODAMPENV,
CARWAVETABLE, MODWAVETABLE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | carrier frequency | Hz | no | no | 
p4 | modulation frequency | Hz | yes | no | 
p5 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 
p6 | modulator amplitude | reference to pfield table-handle | yes | yes | default: 1.0 (full amplitude) | 
p7 | carrier wavetable | reference to pfield table-handle | yes | yes | defaults to sine wave | 
p8 | modulator wavetable | reference to pfield table-handle | yes | yes | defaults to sine wave | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

   Author Brad Garton; rev for v4, JGG, 7/22/04

  

-----

  
**AMINST** synthesizes a sound using amplitude modulation, a type of
modulation where a carrier signal (of frequency *C* is varied by a
modulator signal (of frequency *M*) such that two sidebands are produced
of frequency *C + M* and frequency *C - M*. This results from
multiplying the amplitude of the carrier signal by the amplitude of the
modulator signal. The operation is simple, each instantaneous sample
value of the carrier is multiplied by the corresponding sample value of
the modulator.

At low modulator frequency rates (\< 20 Hz) this is perceived as an
amplitude 'tremolo' of the carrier signal. At higher rates the sidebands
manifest as audio components in a more complex spectrum.

If non-sinusoidal waveforms are used, each sine-wave component acts as a
separate carrier or modulator. Each sine-wave partial in the signal(s)
will have distinct *C + M* and *C + M* components in the resulting
spectrum.

### Usage Notes

Depending on the amplitude of modulator, the original carrier frequency
component will be present in the resulting sound. Negative frequencies
resulting from the AM operation 'wrap' around 0 Hz and appear as
positive components 180 degrees out of phase.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("AMINST")

   // use defaults to produce a signal with components at
   // 178.0 + 315.0 Hz, 178.0 - 315.0 Hz, and 178.0 Hz)
   AMINST(0, 3.5, 10000, 178, 315)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 1)
   load("AMINST")

   ampenv = maketable("line", 1000, 0,0, 1,1, 9,1, 10,0)
   modenv = maketable("line", 1000, 0,0, 1,1, 2,0)
   AMINST(3.9, 3.4, 10000*ampenv, cpspch(8.00), cpspch(8.02), 0, modenv)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("AMINST")

   ampenv = maketable("line", 1000, 0,1, 10,0)
   carfreq = makeLFO("sawup", 0.9, 100, 500)
   modfreq = makeLFO("sawdown", 0.7, 250, 1400)
   carwave = maketable("wave", 1000, "saw3")
   modwave = maketable("wave", 1000, "square")
   panner = makeLFO("sine", 0.5, 0, 1)
   AMINST(0, 7, 20000*ampenv, carfreq, modfreq, panner, 1.0,  carwave, modwave)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeLFO](../scorefile/makeLFO.html), [AM](AM.html),
[FMINST](FMINST.html), [HALFWAVE](HALFWAVE.html),
[MULTIWAVE](MULTIWAVE.html), [SYNC](SYNC.html), [VWAVE](VWAVE.html),
[WAVESHAPE](WAVESHAPE.html), [WAVY](WAVY.html), [WIGGLE](WIGGLE.html)
