---
title: MSHAKERS()
layout: ref
---

## MSHAKERS

"Shaker" PhISEM and PhOLIES physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MSHAKERS**(outsk, dur, AMP, ENERGY, DECAY, NOBJS, RESONANCE,
instrument\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | (seconds) | no | no | 
p1 | duration | (seconds) | no | no | 
p2 | amplitude | (absolute, for 16-bit soundfiles: 0-32768) | yes | no | 
p3 | energy | (0.0-1.0) | yes | no | 
p4 | decay | (0.0-1.0) | yes | no | 
p5 | # of objects | (0.0-1.0) | yes | no | 
p6 | resonance freq | (0.0-1.0) | yes | no | 
p7 | instrument selection | 0-22 | no | no | see the Usage Notes below
p8 | pan | (0-1 stereo; 0.5 is middle) | yes | yes | default is 0.5 | 

   Author:  Brad Garton, based on code from the Synthesis ToolKit

  

-----

  
**MSHAKERS** is the "Shakers" PhISEM and PhOLIES physical model in Perry
Cook and Gary Scavone's
[STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the Synthesis
ToolKit. <span id="usage_notes"></span>

### Usage Notes

**MSHAKERS** realizes the sound of many 'shaken' instruments (maracas,
sleigh bells, change rattling in coffee mugs, etc.) using a "stochastic
modelling" approach that Perry Cook developed. There are a large number
of diverse sounds available through this one instrument -- try 'em all\!

Here's what Perry says about "Shakers":

The instrument number values for p7 ("instrument") are:

Increasing parameter p3 ("ENERGY") will tend to keep the instrument
going. Parameter p4 ("DECAY" interacts with the natural decay of the
shaker. If p4 \> 1.0 the instrument will go for a loooong time (be
careful\!).

**MSHAKERS** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MSHAKERS")

   st = 0
   inst = 0
   for (j = 0; j < 23; j = j+1)
   {
      for (i = 0; i < 7; i = i+1)
      {
         MSHAKERS(st, 0.5, 20000, 0.9, 0.8, 0.5, 0.7, inst)
         st = st + 0.2
      }
      inst = inst + 1
   }
```
  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MSHAKERS")

   st = 0
   numobjs = 0
   for (i = 0; i < 20; i = i+1)
   {
      MSHAKERS(st, 0.5, 30000, 0.8, 0.8, numobjs, 0.1, 0)
      numobjs = numobjs + 0.05
      st = st + 0.2
   }

   st = st + 0.5
   energy = 0
   for (i = 0; i < 20; i = i+1)
   {
      MSHAKERS(st, 0.5, 30000, energy, 0.8, 0.5, 0.1, 0)
      energy = energy + 0.05
      st = st + 0.2
   }

   st = st + 0.5
   decay = 0
   for (i = 0; i < 20; i = i+1)
   {
      MSHAKERS(st, 0.5, 30000, 0.8, decay, 0.5, 0.1, 0)
      decay = decay + 0.05
      st = st + 0.2
   }

   st = st + 0.5
   resofreq = 0
   for (i = 0; i < 20; i = i+1)
   {
      MSHAKERS(st, 0.5, 30000, 0.8, 0.8, 0.5, resofreq, 0)
      resofreq = resofreq + 0.05
      st = st + 0.2
   }
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("MSHAKERS")

   MSHAKERS(0, 3.5, 4*20000, 0.9, 1.05, 0.5, 0.7, 1)

   amp = maketable("line", 1000, 0,0, 1,1, 2,1, 2.1,0)
   MSHAKERS(4, 3.5, amp*5*20000, 0.9, 1.05, 0.5, 0.7, 3)

   energy = maketable("line", "nonorm", 1000, 0,0, 1,0.2, 2,0)
   resonance = makeLFO("tri", 3.5, 0.3, 0.4)
   MSHAKERS(8, 3.5, 5000, energy, 0.1, 0.5, resonance, 14)

   decay = maketable("line", "nonorm", 1000, 0,1, 1,1.1, 2,0)
   pan = maketable("line", 1000, 0,1, 1,0.3, 2,0.7, 3,0, 4,1, 5,0)
   MSHAKERS(12, 3.5, 3*20000, 0.9, decay, 0.5, 0.7, 19, pan)

   nobjs = maketable("line", 1000, 0,1, 1,0, 3,1)
   energy = maketable("line", "nonorm", 1000, 0,0.1, 1,0.2, 2,0)
   MSHAKERS(15, 5.5, 5000, energy, 0.1, nobjs, 0.7, 5)
```

  

-----

### See Also

[AMINST](AMINST.html), [FMINST](FMINST.html),
[MBANDEDWG](MBANDEDWG.html), [MMESH2D](MMESH2D.html),
[MMODALBAR](MMODALBAR.html), [STRUM](STRUM.html), [STRUM2](STRUM2.html),
[STRUMFB](STRUMFB.html)
