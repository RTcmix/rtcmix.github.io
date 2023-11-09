---
title: REVERBIT()
layout: ref
---

## REVERBIT

Modified basic Schroeder reverb.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**REVERBIT**(outsk, insk, dur, AMP, RVBTIME, RVBAMT, chan0delay,
FILTFREQ\[, dcblock, ringdowndur\])

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
p4 | reverb time | seconds, > 0 | yes | no | 
p5 | reverb amount | 0: dry --> 1: wet | yes | no | 
p6 | right channel delay time | seconds, > 0 | no | no | 
p7 | cutoff freq for low-pass filter | Hz, 0 will disable filter | yes | no | 
p8 | apply DC blocking filter | 0: No, 1: Yes | no | yes | default: 1 | 
p9 | ring-down duration | seconds | no | yes | default: p4 value |

   Author:  John Gibson, 6/24/99 rev for v4, 7/11/04
   based on original code by Paul Lansky

  

-----

  
**REVERBIT** reverberates the input signal using a variation on a
typical Schroeder network of comb and allpass filters.

### Usage Notes

**REVERBIT** is a very old reverbator (dates back to the late 1970's).
For a more general reverberation algorithm the [FREEVERB](FREEVERB.html)
or [GVERB](GVERB.html) instruments are probably better. There are also a
number of excellent room-simulator instruments (listed under the [See
Also](#see_also) section below).

Here's how **REVERBIT** works:

  - (1) Runs the input signal into a simple Schroeder reverberator,
    scaling the output of that by the reverb percent and flipping its
    phase. (The reverberator has four comb filters in parallel, followed
    by two allpass filters in series.)  
      
  - (2) Puts output of (1) into a delay line, length determined by
    "chan0delay" (p6).  
      
  - (3) Adds output of (1) to dry signal, and places in left channel.  
      
  - (4) Adds output of delay to dry signal, and places in right
    channel.  
      
  - (4) Feeds output to optional lowpass and DC-blocking filters.

**REVERBIT** works best with short reverb times and a moderate amount of
reverberated sound mixed into the output. If you crank "RVBTIME" (p4)
and "RVBAMT" (p5), you'll hear a grungy, grainy reverb tail. You can
smooth this some using the lowpass filter. Lansky's original intent was
just to "put a gloss on a signal,'' not to swamp it with gushy reverb.

The chan0delay parameter (p6) governs the delay time between output
channels. A small amount (e.g., .02 seconds) livens up the sound; a
larger amount makes an audible slap echo.

The amplitude multiplier (p3, "AMP") is applied to the input sound
before it enters the reverberator.

The point of the optional ring-down duration parameter (p9,
"ringdowndur") is to let you control how long the reverb will ring after
the input has stopped. If the reverb time is constant, **REVERBIT** will
figure out the correct ring-down duration for you. If the reverb time is
dynamic, you must specify a ring-down duration if you want to ensure
that your sound will not be cut off prematurely.

**REVERBIT** requires stereo output. The input file can be either mono
or stereo. If the input is from a real-time source, such as live audio
in or an aux bus, p1 ("inskip") has to be 0.

### Sample Scores

```cpp
   rtsetparams(44100, 2)
   load("NOISE")
   load("REVERBIT")

   bus_config("NOISE", "aux 0 out")
   bus_config("REVERBIT", "aux 0 in", "out 0-1")

   totdur = 7
   for (st = 0; st < totdur; st = st + .3)
      NOISE(st, notedur=.1, amp=20000)

   REVERBIT(outskip=0, inskip=0, totdur + notedur, amp=1, revtime=.6, revpct=.4, rtchandel=.02, cutoff=5000)
```

  

-----

  
<span id="see_also"></span>

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MPLACE](MPLACE.html),
[MROOM](MROOM.html), [PLACE](PLACE.html), [REV](REV.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
