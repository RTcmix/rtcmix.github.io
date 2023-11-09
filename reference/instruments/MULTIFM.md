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
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time |  -  | no | no | 
p1 | duration |  -  | no | no | 
p2 | overall amplitude multiplier |  -  | yes | no | 
p3 | number of oscillators |  -  | no | no | 
p4 | pan | in percent-to-left format | yes | no | 
p5 | oscillator 1 freq | Hz | yes | no | forms pair with next pfield |
p6 | oscillator 1 waveform | reference to pfield table-handle | no | no | forms pair with previous pfield 

##### p5,p6, p7,p8, ..., pN-1, pN form pairs of frequency / waveform values for each oscillator.

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
pN-1 | oscillator N/2 freq | Hz | yes | yes | forms pair with next pfield |
pN | oscillator 1 waveform | reference to pfield table-handle | no | yes | forms pair with previous pfield |
      
##### pO, pO+1, pO+2, ... pP-2, pP-1, pP form triplets for oscillator connections in one of the two following forms:

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
pO | oscillator pair number | 1-N | no | no | 1 refers to the 1st freq / waveform pair, p5-p6. 2 refers to the next pair, and so on
pO+1 | where to direct the output of this oscillator | 0  | no | no | directs signal to audio output
pO+2 | relative amplitude | linear multiple of p2 | yes | no 

##### or

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
pO | oscillator pair number | 1-N | no | no | 1 refers to the 1st freq / waveform pair, p5-p6. 2 refers to the next pair, and so on
pO+1 | where to direct the output of the indicated oscillator | 1-N | no | no | direct it to modulate the frequency of the given oscillator
pO+2 | modulation index | non-negative | yes | no

  Any number of connections in any direction, including feedback, is
  acceptable.

   Author: Neil Thornock (neilthornock at gmail), 11/12/16

  

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

```cpp
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

```cpp
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

```cpp
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
