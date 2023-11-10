---
title: MULTIWAVE()
layout: ref
---

## MULTIWAVE

Wavetable/additive synthesis instrument.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**MULTIWAVE**(outsk, dur, AMP, WAVETABLE, FREQ1, AMP1, phase1, PAN1, ...
FREQN, AMPN, phaseN, PANN)

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
p2 | overall amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | wavetable | reference to a pfield table-handle | no | no | 
p4 | oscillator 1 freq | Hz | yes | no | 
p5 | oscillator 1 amp | relative multiplier of overall amplitude (p2) | yes | no | 
p6 | oscillator 1 initial phase | 0-360 degrees, not updateable | no | no | 
p7 | oscillator 1 pan | 0-1 stereo; 0.5 is middle | yes | no | 

The remaining N pfields are quadruplets describing additional oscillators.
The limit on the number of quadruplets is set by the maximum number of passable arguments (1024).

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
pN-3 | oscillator N/4 freq | Hz | yes | yes | must be part of quadruplet |
pN-2 | oscillator N/4 amp | relative multiplier of overall amplitude (p2) | yes | yes | must be part of quadruplet |
pN-1 | oscillator N/4 initial phase | 0-360 degrees, not updateable | no | yes | must be part of quadruplet |
pN  | oscillator N/4 pan | 0-1 stereo; 0.5 is middle | yes | yes | must be part of quadruplet |

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author: John Gibson, 3/9/05

  

-----

  
**MULTIWAVE** can be used to create a wide range of sounds. Using PField
controls, each individual part can be controlled with a fair degree of
independence, equivalent to an efficient version of many
[WAVETABLE](WAVETABLE.html) notes running simultaneously.

### Usage Notes

This is a fairly straightforward instrument. Because all the fundamental
parameters for each oscillator used may be specified (amp envelope,
phase, pan, frequency (enveloped\!)), true additive synthesis can be
achieved using **MULTIWAVE**. For basic wavetable synthesis, use
[WAVETABLE](WAVETABLE.html).

For rapidly-varying control envelopes, you may want to set the reset
rate higher than the default 1000 times/second. Use the
[reset](../scorefile/reset.html) scorefile command to do this.

**MULTIWAVE** can produce mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MULTIWAVE")

   ampenv = maketable("line", 1000, 0,0, 1,1, 2,0)
   wave = maketable("wave", 1000, 1)

   srand()

   MULTIWAVE(0, 4.3, ampenv*35000, wave,
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random(),
    irand(100, 700), 1, 0, random())
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MULTIWAVE")

   dur = 60
   amp = 15000

   wave = maketable("wave", 2000, 1)
   line = maketable("line", 1000, 0,0, 1,1, 9,1, 10,0)

   rfreq = 1
   freq1 = makerandom("linear", rfreq, min=200, max=1000, seed=1)
   freq1 = makefilter(freq1, "smooth", 90)

   lag = 60
   amp1 = makeconnection("mouse", "Y", 0, 1, 0, lag, "amp") | no | no | 
   pan1 = makeconnection("mouse", "X", 1, 0, .5, lag, "pan")

   MULTIWAVE(0, dur, amp * line, wave,
      freq1, amp1, phase1=0, pan1,
      freq1 * 1.05, amp1, phase1, pan1 * .7)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("MULTIWAVE")
   reset(5000)
   
   ampenv = maketable("line", 1000, 0,0, 1,1, 4,1, 5,0)
   wave = maketable("wave", 1000, 1)
   fadeup = maketable("line", 1000, 0,0, 1,1)
   fadedown = maketable("line", 1000, 0,1, 1,0)
   
   outsk = 0
   dur = 0.21
   for (i = 0; i < 35; i += 1) {
    MULTIWAVE(outsk, dur, ampenv*60000, wave,
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadedown, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random(),
        irand(100, 1400), fadeup, 0, random())
   
    outsk = outsk + dur
   }
```

  
  
more fun stuff\!

```cpp
   rtsetparams(44100, 2, 128)
   load("MULTIWAVE")

   dur = 60
   masteramp = 20000

   minfreq = 50
   maxfreq = 1500
   glide = 50

   // quantize freqs to this number (in Hz); set to zero for no quantum
   //quantum = 100
   quantum = 0

   wave = maketable("wave", 5000, 1)
   line = maketable("line", 1000, 0,0, 1,1, 9,1, 10,0)

   numwaves = 10
   freq = {}
   pan = {}
   for (i = 0; i < numwaves; i += 1) {
      lfofreq = 0.007 + (i * 1.4)
      rfreq = makeLFO("sine", lfofreq, min = 0.2 + (i * 0.03), min * 3.5)
      min = minfreq + (i * 10)
      max = maxfreq - (i * 70)
      rand = makerandom("linear", rfreq, min, max, seed = i + 1)
      freq[i] = makefilter(rand, "smooth", glide)
      if (quantum)
         freq[i] = makefilter(freq[i], "quantize", quantum)
      min = mod(i, 2)
      if (min == 0)
         max = 1
      else
         max = 0
      pan[i] = makeLFO("sine", 0.007 + (i * 0.026), min, max)
   }

   amp = 1
   phase = 0

   MULTIWAVE(0, dur, masteramp * line, wave,
      freq[0], amp, phase, pan[0],
      freq[1], amp, phase, pan[1],
      freq[2], amp, phase, pan[2],
      freq[3], amp, phase, pan[3],
      freq[4], amp, phase, pan[4],
      freq[5], amp, phase, pan[5],
      freq[6], amp, phase, pan[6],
      freq[7], amp, phase, pan[7],
      freq[8], amp, phase, pan[8],
      freq[9], amp, phase, pan[9])
```

  
  
even *more* fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("MULTIWAVE")

   dur = 60
   masteramp = 30000

   // in linear octaves
   minpitch = 6.00
   maxpitch = 10.00

   glide = 50

   wave = maketable("wave", 5000, 1, 0, .02)
   line = maketable("line", 1000, 0,0, 1,1, 8,1, 10,0)

   pitchtable = maketable("literal", "nonorm", 0,
      6.00,
      7.00,
      octpch(7.05),
      octpch(7.07),
      8.00,
      octpch(8.07),
      octpch(8.08),
      9.00,
      octpch(9.07)
   )

   numwaves = 10
   freq = {}
   amp = {}
   pan = {}
   for (i = 0; i < numwaves; i += 1) {
      lfofreq = 0.007 + (i * 1.4)
      rfreq = makeLFO("sine", lfofreq, min = 0.2 + (i * 0.03), min * 3.5)
      min = minpitch + (i * octpch(0.03))
      max = maxpitch - (i * octpch(0.02))
      freq[i] = makerandom("low", rfreq, min, max, seed = i + 2)
      freq[i] = makefilter(freq[i], "constrain", pitchtable, 0.95)
      freq[i] = makefilter(freq[i], "smooth", glide)
      freq[i] = makeconverter(freq[i], "cpsoct")
      min = i % 2
      if (min == 0)
         max = 1
      else
         max = 0
      amp[i] = makeLFO("sine", 0.06 + (i * 0.04), 0.2, 1)
      pan[i] = makeLFO("sine", 0.007 + (i * 0.026), min, max)
   }

   phase = 0

   MULTIWAVE(0, dur, masteramp * line, wave,
      freq[0], amp[0], phase, pan[0],
      freq[1], amp[1], phase, pan[1],
      freq[2], amp[2], phase, pan[2],
      freq[3], amp[3], phase, pan[3],
      freq[4], amp[4], phase, pan[4],
      freq[5], amp[5], phase, pan[5],
      freq[6], amp[6], phase, pan[6],
      freq[7], amp[7], phase, pan[7],
      freq[8], amp[8], phase, pan[8],
      freq[9], amp[9], phase, pan[9])
```

  

-----

### See Also

[makeconverter](../scorefile/makeconverter.html),
[makefilter](../scorefile/makefilter.html),
[makeLFO](../scorefile/makeLFO.html),
[makerandom](../scorefile/makerandom.html),
[maketable](../scorefile/maketable.html),
[cpspch](../scorefile/cpspch.html), [AMINST](AMINST.html),
[FMINST](FMINST.html), [HALFWAVE](HALFWAVE.html), [VWAVE](VWAVE.html),
[SYNC](SYNC.html), [WAVESHAPE](WAVESHAPE.html),
[WAVETABLE](WAVETABLE.html), [WAVY](WAVY.html), [WIGGLE](WIGGLE.html)
