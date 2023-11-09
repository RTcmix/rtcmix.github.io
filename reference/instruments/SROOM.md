---
title: SROOM()
layout: ref
---

## SROOM

Simple stationary-source room simulation.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**SROOM**(outsk, insk, dur, amp, rightdist, frontdist, xloc, yloc,
rvbtime, reflect, inroomwidth\[, inputchan\])


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | no | no | 
p4 | distance from middle of room to right wall | feet | no | no | 
p5 | distance from middle of room to front wall | feet | no | no | 
p6 | x position of sound source | feet | no | no | 
p7 | y position of source | feet | no | no | 
p8 | reverb time | seconds | no | no | 
p9 | reflectivity | 0 - 100; the higher, the more reflective | no | no | 
p10 | "inner room" width | feet; try 8 | no | no | 
p11 | input channel number |  -  | no | yes | default: 0 | 


NOTE: This is an older RTcmix instrument, the newer
[MPLACE](MPLACE.html) instrument is probably better to use for
room-simulation.  
  

-----

  
**SROOM** uses the F. R. Moore approach to modelling room acoustics
(described in F.R. Moore: "A General Model for Spatial Processing of
Sounds." The Computer Music Journal, Vol 7/3, pp. 6-15, 1983). you to
specify a trajectory of a source sound through the room. NOTE: This is
an older RTcmix instrument, the newer [MPLACE](MPLACE.html) or
[PLACE](PLACE.html) instruments do a better job of room-simulation,
although they are more computationally expensive.
<span id="usage_notes"></span>

### Usage Notes

**SROOM** will add reverberation and "roominess" to sounds it processes.
Delay lines are calculated from the source to the listener. The room
model is rectangular, delay paths are based on "ideal" reflections from
the walls, the \`listener' (or "inner room" in Moore's conception) has
an expandable head, and reverberation is added to the delay lines to
simulate diffusion from reflections of reflections. (NOTE: This is where
[MPLACE](MPLACE.html) or [PLACE](PLACE.html) may be better then
**SROOM**. The reverberation algorithm employed by
[MPLACE](MPLACE.html)/[PLACE](PLACE.html) is much better than the simple
Schroeder model used in **SROOM**

**SROOM** is identical to [MROOM](MROOM.html) except that
[MROOM](MROOM.html) has the ability to process a moving sound source.

The room is set up on a Cartesian coordinate system (x-y plane) with the
center of the listener's head ("inner room" front wall) positioned at
the origin (0,0).

Parameter p4 ("rightdist") is the position along the x-axis where the
right-hand wall should appear. **SROOM** will then create the left-hand
wall at -p4, so this value represents 1/2 the room's width (in feet).
Similarly, p5 ("frontdist") is the position along the y-axis where the
front wall will be drawn. The rear wall will be positioned at -p5. p5
represents 1/2 the room's height (also in feet).

The parameters p6 and p7 ("xloc" and "yloc") are the x and y coordinates
of the sound source. "rvbtime" (p8) is the amount of time (in seconds)
it takes for the reverb comb filters to decay to .001 of their original
value. Values greater than 1.0 - 1.5 tend to sound "metallic"; the
frequency response of the reverb combs becomes clearly audible. p9
("reflect") is the percentage of sound reflected by the walls. A p9
value of 100 will mean that the walls will not absorb any of the
incident sound. The only attenuation in the delay paths will be due to
the distance travelled from the source to the listener (-6 db for each
doubling of distance). p10 ("inroomwidth") is the width of the inner (or
listening) room (or the size of the listener's head). The value
represents 1/2 the true width of the inner room (in feet). p11
("inputchan") is an optional argument to specify which channel of the
input sound to process through the room. The default is channel 0. Note
that only one channel at a time may be processed through **SROOM**

The older [makegen](../scorefile/makegen.html) control envelope sysystem
can be used to place an amplitude envelope in function slot 1 (the
[setline](../scorefile/setline.html) scorefile command may also be used
to do this).

**SROOM** produces stereo output only.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2)
   load("SROOM")

   rtinput("mysound.snd")

   outskip = 0
   inskip = 0
   dur = DUR()
   amp = 0.8
   xdim = 20
   ydim = 20
   xsrc = 10
   ysrc = 10
   rvbtime = 1.0
   reflect = 90.0
   innerwidth = 4.0
   inchan = 0

   SROOM(outskip, inskip, dur, amp, xdim, ydim, xsrc, ysrc, rvbtime, reflect, innerwidth, inchan)

   outskip = dur+1
   amp = 0.7
   ydim = 50
   xsrc = -30
   reflect = 80.0

   SROOM(outskip, inskip, dur, amp, xdim, ydim, xsrc, ysrc, rvbtime, reflect, innerwidth, inchan)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MPLACE](MPLACE.html),
[MROOM](MROOM.html), [PLACE](PLACE.html), [REV](REV.html),
[REVERBIT](REVERBIT.html), [ROOM](ROOM.html)
