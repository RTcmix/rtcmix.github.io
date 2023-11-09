---
title: STEREO()
layout: ref
---

## STEREO

Mix input to stereo output with adjustable pan.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**STEREO**(outsk, insk, dur, AMP, P4-N: input/output channel pan
assigns)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.
  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | seconds | no | no | endtime if negative |
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4-n | input/output channel pan assigns | 0-1 stereo; 0.5 is middle | yes | no | 

   Author:  Brad Garton; rev. for v4.0 by JGG, 7/9/04

  

-----

  
**STEREO** will mix any number of input channels to stereo outputs with
global amplitude control and individual pans for each input channel.

### Usage Notes

**STEREO** mixes channels from the input source into the stereo output,
according to the 'input/output channel pan assign' in the p4-n pfields.
The total number ("n") of these pfields will depend on how many channels
are present in the input. These pfields together constitute a 'mix
matrix', in which argument position represents input channel number, and
argument value represents output stereo location. These output locations
are expressed as a 0-1 value, with 0.5 being in the middle (0 == left
channel, 1 == right channel.

It works like this: For every input channel, the corresponding number in
the pfield (starting with p4) gives the output stereo pan for that
channel (0-1, 0.5 in middle). Thus p4 corresponds to input channel 0, p5
corresponds to input channel 1, etc. If the value of one of these
pfields is negative, then the corresponding input channel will not be
played. Note that you cannot send a channel to more than one output pan
location.

Each of these individual input pan values can received dynamic updates
through the [pfield-enabled](pfield-enabled.html) control system.

**STEREO** requires stereo output (obviously).

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("STEREO")

   rtinput("mysoundfile.aif")

   loc = 0.75
   STEREO(outskip=0, inskip=1, dur=3, amp=1, loc)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("STEREO")

   rtinput("mysound.aif")

   ampenv = maketable("line", 1000, 0,0, 1,1, 1.1, 0)
   STEREO(0, 0, 3.5, 0.7*ampenv, 0.5, 0.5)

   ampenv = maketable("line", 1000, 0,0, 0.1,1, 1, 0)
   STEREO(2, 0, 3.5, 0.6*ampenv, 0.1, 0.1)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("STEREO")

   rtinput("mysound.aif")

   ampenv = maketable("line", 1000, 0,0, .1,1, 2,0)

   reset(44100)
   dur = 1
   for(outsk = 0; outsk < 10.0; outsk = outsk + dur) {
      insk = random() * 7.0
      dur = random() * 0.2
      STEREO(insk, outsk, dur, 1, random(), -1)
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [MIX](MIX.html),
[NPAN](NPAN.html), [PAN](PAN.html), [QPAN](QPAN.html),
[REVMIX](REVMIX.html), [SPLITTER](SPLITTER.html)
