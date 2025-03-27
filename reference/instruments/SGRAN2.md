---
title: SGRAN2()
layout: ref
---

## SGRAN2

Stochastic granular synthesizer

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**SGRAN2**(outsk, dur, AMP, RATELOW, RATEMID, RATEHIGH, RATETIGHT,DURLOW, DURMID, DURHIGH, DURTIGHT,FREQLOW, FREQMID,FREQHIGH, FREQTIGHT, PANLOW, PANMID, PANHIGH, PANTIGHT, WAVETABLE, GRAINENV, \[, maxgrains\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field    | Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | total duration | seconds | no | no | 
p2 | amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | grain rate low | seconds per grain | yes | no | 
p4 | grain rate mid | seconds per grain | yes | no | 
p5 | grain rate high | seconds per grain | yes | no | 
p6 | grain rate tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p7 | grain dur low | seconds | yes | no | 
p8 | grain dur mid | seconds | yes | no | 
p9 | grain dur high | seconds | yes | no | 
p10 | grain rate tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p11 | grain freq low | Hz or oct.pc | yes | no | 
p12 | grain freq mid | Hz or oct.pc  | yes | no | 
p13 | grain freq high | Hz or oct.pc  | yes | no | 
p14 | grain freq tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p15 | grain pan low | 0.0-1.0 | yes | no | 
p16 | grain pan mid | 0.0-1.0  | yes | no | 
p17 | grain pan high | 0.0-1.0  | yes | no | 
p18 | grain pan tight | - | yes | no |  >1, tighter preference, btwn 0 and 1, inverted preference |
p19 | wavetable | reference to pfield table-handle | yes | no | 
p20 | grain amp env | reference to pfield table-handle | yes | no | 
p21 | max grain limit | number | no | yes | default: 1500 |

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Kieran McAuliffe, 2023.

  

-----

  
**SGRAN2** is a granular synthesizer, with pfield control and improved performance after Mara Helmuth's SGRANR ([SGRANR](SGRANR.html)) and original sgran. 


## Usage

- p3-18 A `prob` function, which takes four floating point parameters: `low`, `mid`, `high` and `tight`, is used to create stochastic values for the parameters for each grain.  Calling this function returns a stochastically chosen value based on a distribution centered around `mid` with upper and lower bounds at `low` and `high`.  The `tight` value determines how closely the distribution clusters at `mid`.  `tight` of 1 will be an even distribution, with more than one being closer to the `mid` value, and less than one spreading towards the `low` and `high` bounds. Every time a new grain spawns, four `prob` functions run to generate the rate, duration, frequency and pan values of that grain. p3-p6 are the low, mid high and tight prob input values for grain rate; p7-p10 are the corresponding values for grain duration; p11-p14 are the low, mid, high and tight values for grain frequency in frequency or octave point pitch class format, and p15-p18 are the values for grain pan, from 0.0 to 1.0.  

- p19 oscillator waveform table used in each grain.

- p20 pfield-handle reference to a table with the
    envelope to be applied for each grain. 
    
- p21 maximum simultaneous number of grains allowed. 

**SGRAN2** can produce either stereo or mono output. 

### Sample Score

```cpp
   rtsetparams(44100, 2)
   load("SGRAN2")

   outskip = 0
   dur = 15

   amp = maketable("line", 1000, 0, 0, 8, 0.8, 16, 1, 17, 0)

   ratelo = 0.004
   ratemid = 0.005
   ratehi = 0.007
   rateti = maketable("line", "nonorm", 200, 0, 8, 1, 0.2)

   durlo = 0.01
   durmid = 0.05
   durhi = 0.08
   durti = 0.1

   freqlo = maketable("line", "nonorm", 200, 0, 400, 1, 200)
   freqmid = maketable("line", "nonorm", 200, 0, 430, 1, 350, 2, 600)
   freqhi = maketable("line", "nonorm", 200, 0, 440, 1, 460, 2, 800)
   freqti = maketable("line", "nonorm", 200, 0, 6, 1, 0.2)

   panlo = 0
   panmid = maketable("line", "nonorm", 200, 0, 0.1, 1, 0.1, 2, 0.5)
   panhi = maketable("line", "nonorm", 200, 0, 0.2, 1, 0.5, 2, 1)
   panti = 0.4

   wave = maketable("wave", 1000, "square")
   env = maketable("window", 1000, "hanning")

   SGRAN2(outskip, dur, 800 * amp, ratelo, ratemid, ratehi, rateti, durlo, durmid, durhi, durti, 
   freqlo, freqmid, freqhi, freqti, panlo, panmid, panhi, panti, wave, env)
```

### Examples

[First](https://user-images.githubusercontent.com/69212477/147691785-44a433a8-5641-47cd-8736-3a59bc73df5a.mp4)

[Second](https://user-images.githubusercontent.com/69212477/147691891-53d72308-b080-4f00-8393-49e684ce733b.mp4)


-----

### See Also

[SGRANR](SGRANR.html),[STGRANR](STGRANR.html), [GRANSYNTH](GRANSYNTH.html), [JGRAN](JGRAN.html), [GRANULATE](GRANULATE.html), [JCHOR](JCHOR.html),
