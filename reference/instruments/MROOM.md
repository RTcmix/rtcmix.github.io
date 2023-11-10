---
title: MROOM()
layout: ref
---

## MROOM

Simple moving-source room simulation.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**timeset**(time, xloc, yloc)  
  
**MROOM**(outsk, insk, dur, amp, rightdist, frontdist, rvbtime, reflect,
inroomwidth\[, inputchan, updaterate\])

-----

  
**MROOM** uses a subcommand (**timeset**) to specify the sound source
trajectory.  
  
  
<span id="timeset"></span> **timeset**  
  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | timepoint | seconds | no | no | 
p1 | x location | feet, Cartesian coordinates | no | no | 
p2 | y location | feet, Cartesian coordinates | no | no | 

  
<span id="MROOM"></span> **MROOM**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | no | no | 
p4 | distance from middle of room to right wall | feet; i.e., 1/2 of width | no | no | 
p5 | distance from middle of room to front wall | feet; i.e., 1/2 of depth | no | no | 
p6 | reverb time | seconds | no | no | 
p7 | reflectivity | 0 - 100; the higher, the more reflective | no | no | 
p8 | "inner room" width | feet; try 8 | no | no | 
p9 | input channel number |  -  | no | yes | default: 0 | 
p10 | control rate for trajectory |  -  | no | yes | default: 100 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Assumes function table 1 is the amplitude envelope.

  

-----

  
**MROOM** uses the F. R. Moore approach to modelling room acoustics
(described in F.R. Moore: "A General Model for Spatial Processing of
Sounds." The Computer Music Journal, Vol 7/3, pp. 6-15, 1983), and
allows you to specify a trajectory of a source sound through the room.

NOTE: This is an older RTcmix instrument, the newer [MMOVE](MMOVE.html)
or [DMOVE](DMOVE.html) (pfield-enabled data specification) instruments
are probably better to use, although they are more computationally
expensive. <span id="usage_notes"></span>

### Usage Notes

The trajectory of the sound source in **MROOM** is mapped using the
subcommand **timeset** repeatedly::

where *timepoint* determines at what time in the total note duration
(given in seconds) that the source should be at point (*x-location,
y-location*). These locations are in feet using Cartesian coordinates
with the 'front wall' of the 'inner room' constructed along the x-axis)
to be reached by the source sound at that time. Up to 100 of these
points may be specified.

**timeset** is called repeatedly to create the trajectory for the sound
source through the room. Be sure that the timepoints are in ascending
order\!

**MROOM** then adds reverberation and "roominess" to a sound source,
calculating the delay lines from the source located in an 'outer room'
to an 'inner room'. The room model is rectangular, delay paths are based
on "ideal" reflections from the walls, the 'listener' ('inner room') has
an exandable head, and reverberation is added to the delay lines to
simulate diffusion from reflections of reflections (NOTE: This is where
[MMOVE](MMOVE.html) or [DMOVE](DMOVE.html) may be better then **MROOM**.
The reverberation algorithm employed by
[MMOVE](MMOVE.html)/[DMOVE](DMOVE.html) is much better than the simple
Schroeder model used in **MROOM**

**MROOM** is identical to [SROOM](SROOM.html) except for the ability to
process a moving sound source.

The delay paths are calculated from the walls of a rectangular room to
two points representing the corners of an 'inner' or listening room. The
room is set up on a Cartesian coordinate system (x-y plane) with the
center of the inner room's front wall positioned at the origin (0,0).

The variable "rightdist" (p4) is the position along the x-axis where the
right-hand wall should appear. **MROOM** will then create the left-hand
wall at "-rightdist", so this value represents 1/2 the room's width (in
feet). "frontdist" (p5) is the position along the y-axis where the front
wall will be drawn. The rear wall will be positioned at "-frontdist".
"frontdist" represents 1/2 the rooms length (also in feet).

"rvbtime" (p6) is the amount of time (in seconds) it takes for the
reverb comb filters to decay to .001 of their original value. Values
greater than 1.0 - 1.5 tend to sound "metallic"; the frequency response
of the reverb combs becomes clearly audible.

The parameter p7 ("reflect") is the percentage of sound reflected by the
walls. A p7 value of 100 will mean that the walls will not absorb any of
the incident sound. The only attenuation in the delay paths will be due
to the distance travelled from the source to the listener (-6 db for
each doubling of distance).

The "inroomwidth" (p8) parameter sets width of the inner (or listening)
room. The value represents 1/2 the true width of the inner room (in
feet).

**MROOM** will calculate the doppler shift for the source as it moves
relative to the listening room and the walls -- just like the real
world\! The amount of frequency shift from the doppler effect for
sources moving directly towards or away from a listener (or a wall) can
be calculated by:

where c = speed of sound (1086 ft/sec); F(old) is the original source
frequency; SV = source velocity (ft/sec); and F(new) is the resultant
doppler-shifted frequency.

Very fast source velocities may generate some quantization noise if the
position of the sound source is not updated frequently enough. **MROOM**
has an optional argument "updaterate" (p10) that specifies the number of
times/second to update the source position. The default value is 100.
Values greater than this will cause **MROOM** to execute proportionally
slower. This value is independent of the pfield-parameter control rate
set by the [reset](../scorefile/reset.html) scorefile command.

As an example of how this works, consider the following scorefile:

```cpp
   timeset(0, 17, 19)
   timeset(17, -10, 15)
   timeset(29, -11, -7)
   timeset(49, -20, -14)
   timeset(57, -19, -37)
   timeset(77, 14,- 5)

   MROOM(14, 0.7, 77, 1, 21, 49, .9, 50, .5)
```

The call to **timeset** specify a trajectory for the sound source
through the room that looks approximately like this (each 'X' represents
a timepoint in the **timeset** specification:

```cpp

                                 |+49
                                 |
                                 |
                                 |
                                 |
                                 |
                                 |
                     _X-----<----|-<------<------<----X  <--Start Here
                    /            |
     ______________|_____________|_____________________________
     -21         __X             |                  X       +21
            ____/                |           ______/
         __/                     |      ____/
        X                        |_____/
        |                  ______/
        |          _______/      |
        |   ______/              |
        |__/                     |
        X                        |
                                 |-49
```

**MROOM** requires stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2)
   load("MROOM")

   rtinput("mysound.snd")

   outskip = 0
   inskip = 0
   dur = DUR()
   amp = 0.6
   xdim = 30
   ydim = 80
   rvbtime = 1.0
   reflect = 90.0
   innerwidth = 8.0
   inchan = 0
   quant = 2000

   timeset(0, 0-xdim, 0-ydim)
   timeset(dur/2, xdim/8, ydim/8)
   timeset(dur, xdim, ydim)
   
   makegen(1, 24, 1000, 0,0, dur/8,1, dur-.5,1, dur,0)
   
   MROOM(outskip, inskip, dur, amp, xdim, ydim, rvbtime, reflect, innerwidth, inchan, quant)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MPLACE](MPLACE.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
