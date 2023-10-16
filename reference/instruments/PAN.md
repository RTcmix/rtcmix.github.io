---
title: PAN()
layout: ref
---

## PAN

Stereo panning of input signal.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**PAN**(outsk, insk, dur, AMP, inputchan, PANMODE, PANENV)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = input channel
   p5 = 0: use constant-power panning, 1: use linear panning
   p6 = pan (0-1 stereo; 0.5 is middle)

   p3 (amplitude), p5 (pan mode) and p6 (pan) can receive dynamic updates from
   a table or real-time control source.

   Author:  John Gibson, 1/26/00; rev for v4, JGG, 7/24/04
```

  

-----

  
**PAN** is a simple mixing instrument that follows a pan curve

### Usage Notes

**PAN** lets you pan the input sound continuously, according to the
control values coming through p6 ("PANENV"). These control values are
interpreted on a 0-1 scale, with 0.5 being set in the middle of the
stereo output.

**PAN** uses "constant-power'' panning to prevent a sense of lost power
when the sound source moves toward the center, unless you set p5
("PANMODE") to zero. Sometimes this causes jerkey panning motion near
hard left/right, so you can defeat it bys setting p5 to 1.

For rapidly-varying pan control envelopes, you may want to set the reset
rate higher than the default 1000 times/second. Use the
[reset](../scorefile/reset.html) scorefile command to do this.

The output of **PAN** is stereo only.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("PAN")

   rtinput("mysound.aif")

   dur = DUR()

   amp = 1.0
   ampenv = maketable("line", 1000, 0,0, 1,1, dur-1,1, dur,0)

   panenv = maketable("line", 1000, 0,1, 1,0, 3,.5)

   PAN(outskip=0, inskip=0, dur, amp*ampenv, inchan=0, panmode=0, panenv)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("WAVETABLE")
   load("PAN")

   /* feed wavetable into panner */
   bus_config("WAVETABLE", "aux 0 out")
   bus_config("PAN", "aux 0 in", "out 0-1")

   reset(10000)  /* lower control rates can produce zipper noise */

   dur = 10.0
   amp = 25000
   ampenv = maketable("line", 1000, 0,0, 1,1, 7,1, 10,0)
   pitch = 8.00
   wavetable = maketable("wave", 1000, 1, .4, .3, .2, .1)

   WAVETABLE(0, dur, amp*ampenv, pitch, 0, wavetable)

   panenv = makeLFO("tri", 0.5, 0, 1)   // pan once every 2 seconds

   PAN(0, 0, dur, amp=1, 0, 1, panenv)   /* disable constant-power panning */
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeLFO](../scorefile/makeLFO.html), [DMOVE](DMOVE.html),
[MIX](MIX.html), [MMOVE](MMOVE.html), [MOVE](MOVE.html),
[MPLACE](MPLACE.html), [MROOM](MROOM.html), [NPAN](NPAN.html),
[PLACE](PLACE.html), [QPAN](QPAN.html), [ROOM](ROOM.html),
[SROOM](SROOM.html) [STEREO](STEREO.html)
