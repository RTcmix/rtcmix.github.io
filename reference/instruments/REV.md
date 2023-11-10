---
title: REV()
layout: ref
---

## REV

Several basic reverberators.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**REV**(outsk, insk, dur, AMP, rvbtype, rvbtime, RVBAMT\[, inputchan\])

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
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | reverb type | integer - 1: PRCRev, 2: JCRev, 3: NRev | no | no | see Usage Notes below |
p5 | reverb time | seconds | no | no | 
p6 | reverb amount | 0: dry --> 1: wet | yes | no | 
p7 | input channel |  -  | no | yes | default: 0 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  John Gibson, 7/19/99; rev for v4, 7/21/04
based on several reverberators from the STK package (by Perry Cook, Gary Scavone, and Tim Stilson)

  

-----

  
**REV** processes input audio through a choice of three different reverb
types (all from [CCRMA](http://www-ccrma.stanford.edu/) st Stanford
University). <span id="usage_notes"></span>

### Usage Notes

The reverberators in **REV** all have characteristic 'sounds', for more
general reverberation the [FREEVERB](FREEVERB.html) or
[GVERB](GVERB.html) instruments are probably better. There are also a
number of excellent room-simulator instruments (listed under the [See
Also](#see_also) section below). The **REV** algorithms are very
efficient, though.

The reverb types are:  

```cpp
     (1) PRCRev (Perry R. Cook)
           2 allpass units in series followed by 2 comb filters in parallel.

     (2) JCRev (John Chowning)
           3 allpass filters in series, followed by 4 comb filters in
           parallel, a lowpass filter, and two decorrelation delay lines
           in parallel at the output.

     (3) NRev (Michael McNabb)
           6 comb filters in parallel, followed by 3 allpass filters, a
           lowpass filter, and another allpass in series, followed by 2
           allpass filters in parallel with corresponding right and left
           outputs.
```

**REV** can produce either mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("REV")
   
   rtinput("mysound.aif")

   outskip = 0
   inskip = 0
   dur = DUR()
   amp = 0.7
   rvbtype = 3
   rvbtime = 0.3
   rvbpct = 0.3
   inchan = 0

   ampenv = maketable("line", 1000, 0,0, 1,1, dur-1,1, dur,0)

   REV(outskip, inskip, dur, amp*ampenv, rvbtype, rvbtime, rvbpct, inchan)
```

  

-----

  
<span id="see_also"></span>

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MPLACE](MPLACE.html),
[MROOM](MROOM.html), [PLACE](PLACE.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
