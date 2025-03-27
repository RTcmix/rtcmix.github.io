---
title: STGRAN2()
layout: ref
---

## STGRAN2

Stochastic granular sampling.

*in RTcmix/insts/std/SGRAN2*  
  

-----

##### quick syntax:

**STGRAN2**(outsk, INSK, dur, AMP, RATELOW, RATEMID, RATEHIGH, RATETIGHT,DURLOW, DURMID, DURHIGH, DURTIGHT, TRANSPLOW, TRANSPMID, TRANSPHIGH, TRANSPTIGHT, PANLOW, PANMID, PANHIGH, PANTIGHT, GRAINENV, buffersize, \[, maxgrains\, input channel])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field    | Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | total duration | seconds | no | no | 
p3 | amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p4 | grain rate low | seconds per grain | yes | no | 
p5 | grain rate mid | seconds per grain | yes | no | 
p6 | grain rate high | seconds per grain | yes | no | 
p7 | grain rate tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p8 | grain dur low | seconds | yes | no | 
p9 | grain dur mid | seconds | yes | no | 
p10 | grain dur high | seconds | yes | no | 
p11 | grain rate tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p12 | grain transp low | oct.pc | yes | no | 
p13 | grain transp mid | oct.pc  | yes | no | 
p14 | grain transp high | oct.pc  | yes | no | 
p15 | grain transp tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p16 | grain pan low | 0.0-1.0 | yes | no | 
p17 | grain pan mid | 0.0-1.0  | yes | no | 
p18 | grain pan high | 0.0-1.0  | yes | no | 
p19 | grain pan tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p20 | grain amp env | reference to pfield table-handle | yes | no | 
p21 | buffer size | samples | no | yes  |
p22 | max grain limit | number | no | yes | default: 1500 |
p23 | input channel | number | no | yes | default: 0 |

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Kieran McAuliffe, 2023.

  

-----

  
**STGRAN2** is a granular synthesizer, with pfield control and improved performance after Mara Helmuth's STGRANR ([STGRANR](STGRANR.html)) and original stgran. STGRAN2 works with a provided soundfile or live input signal, and has configurable input buffer features.


## Usage

- p3-18 A `prob` function, which takes four floating point parameters: `low`, `mid`, `high` and `tight`, is used to create stochastic values for the parameters for each grain.  Calling this function returns a stochastically chosen value based on a distribution centered around `mid` with upper and lower bounds at `low` and `high`.  The `tight` value determines how closely the distribution clusters at `mid`.  `tight` of 1 will be an even distribution, with more than one being closer to the `mid` value, and less than one spreading towards the `low` and `high` bounds. Every time a new grain spawns, four `prob` functions run to generate the rate, duration, transposition and pan values of that grain. p4-p7 are the low, mid high and tight prob input values for grain rate; p8-p11 are the corresponding values for grain duration; p12-p15 are the low, mid, high and tight values for grain frequency in octave point pitch class format, and p16-p19 are the values for grain pan, from 0.0 to 1.0.*  

- p20 pfield-handle reference to a table with the
    envelope to be applied for each grain.** 
    
- p21 size of the buffer used to choose grain start points [optional; default is 1]*

- p22 maximum concurrent grains [optional; default is 1500]

- p23: input channel [optional; default is 0]

\* may receive a reference to a pfield handle  

\*\* must receive a reference to a pfield maketable handle

**STGRAN2** can produce either stereo or mono output. 

### Sample Score

```cpp
   rtsetparams(44100, 2)
   rtinput("../../../snd/loocher.aiff")
   load("STGRAN2")

   outskip = 0
   inskip = 0
   dur = 12.0
   amp = maketable("line", 1000, 0, 0, 1, 1, 20, 1, 21, 0)

   ratelo = 0.000004
   ratemid = 0.0001
   ratehi = .004
   rateti = 0.6 

   durlo = 0.02
   durmid = 0.03
   durhi = 0.1
   durti = 1

   translo = -1.00
   transmid = 0
   transhi = 1.00
   transti = 2

   panlo = 0
   panmid = 0.5
   panhi = 1
   panti = 0.6

   env = maketable("window", 1000, "hanning") // grain env

   buffer_size = makeLFO("square", .5, 0.02, 1) // audio buffer

   STGRAN2(outskip, inskip, dur, 0.2 * amp, ratelo, ratemid, ratehi, 
   rateti, durlo, durmid, durhi, durti, translo, transmid, transhi, 
   transti, panlo, panmid, panhi, panti, env, buffer_size)
```

### Examples

[First](https://user-images.githubusercontent.com/69212477/148407993-227b08c9-a545-46cc-b253-875c567a8963.mp4)

[Second](https://user-images.githubusercontent.com/69212477/148408034-002d62c7-b3ef-4b4c-9067-4aa3a63c321d.mp4)


-----

### See Also

[SGRANR](SGRANR.html),[STGRANR](STGRANR.html), [GRANSYNTH](GRANSYNTH.html), [JGRAN](JGRAN.html), [GRANULATE](GRANULATE.html), [JCHOR](JCHOR.html),
