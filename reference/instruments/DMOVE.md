---
title: DMOVE()
layout: ref
---

## DMOVE

Pfield-enabled moving source room-simulation.

*in RTcmix/insts/std/MMOVE*  
  

-----

##### quick syntax:

**DMOVE**(outsk, insk, dur, AMP, DIST-XPOS, ANGLE-YPOS,
(-)dist\_mikes\[, inchan\]);  
  
**RVB**(outsk, insk, dur, AMP)  
  
**space**(front, right, -back, -left, ceiling, absorb, rvbtime)  
  
**threshold**(update\_rate)  
  
**mikes**(mike\_angle, pattern)  
  
**mikes\_off**()  
  
**set\_attenuation\_params**(min\_dist, max\_dist, dist\_exponent)  
  
**matrix**(amp, matrixval1, matrixval2, ... matrixval144)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**DMOVE** employs several subcommands to set the room-simulation
characteristics and one sub-instrument (**RVB**) to operate.

NOTE: Although the older path-trajectory subcommands may be used in this
instrument (**path**, **cpath**, **param**, **cparam**, see the
[MMOVE](MMOVE.html) documentation for a description of these), it is
intended for this information to be specified in the "DIST-XPOS" and
"ANGLE-YPOS" pfield parameters.  

For all commands below: Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

  
<span id="DMOVE"></span> **DMOVE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration (or endtime if negative) | seconds | no | no |
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | distance to, or x-coordinate of, sound source | feet* | yes | no | * p6 < 0: cartesian, else polar | 
p5 | angle to, or y-coordinate of, sound source | feet* or degrees (0 is straight in front) | yes | no | * p6 < 0: cartesian, else polar
p6 | distance between 'mics' (stereo receivers) in the room | feet | no | no | if negative, p4/p5 will be interpreted as x- and y- coordinates, otherwise p4/p5 will set polar coordinates
p7 | input channel |  -  | no | yes | default: 0 | 

  
<span id="RVB"></span> **RVB**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration (or endtime if negative) | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 

   NOTE: this associated instrument is required for DMOVE to function

  
<span id="space"></span> **space**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | distance to front wall of room | feet | no | no | 
p1 | distance to right-hand wall of room | feet | no | no | 
p2 | distance to back wall of room | feet | no | no | must be specified as negative distance | 
p3 | distance to left-hand wall of room | feet | no | no | must be specified as negative distance | 
p4 | distance to ceiling of room | feet | no | no | 
p5 | wall absorption factor | 0-10 | no | no | 0 == more 'dead', 10 == more 'live'
p6 | reverberation time | seconds | no | no | 0 - ~5 work best

   NOTE: this subcommand is required for DMOVE to function

  
<span id="threshold"></span>**threshold**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | time interval | seconds | no | no | for trajectory update | 

   NOTE: this subcommand is optional for MMOVE to function. Default is RTBUFSAMPS/SR (both set in rtsetparams).

  
<span id="mikes"></span> **mikes**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | microphone angle | degrees, 0 degrees is straight in front | no | no | 
p1 | microphone pattern | 0-1 | no | no | 0 == omnidirectional, 1 == highly directional

   NOTE: this subcommand is optional for DMOVE to function (the default is "mikes_off")

  
<span id="mikes_off"></span> **mikes\_off**  

No pfields, this defeats the microphone angle and pattern settings for binaural simulation

   NOTE: this subcommand is optional for DMOVE to function

  
<span id="set_attenuation_params"></span> **set\_attenuation\_params**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | minimum distance | feet | no | no | default: 0
p1 | maximum distance | feet | no | no | default: infinite
p2 | distance attentuation exponent |  -  | no | no | default: 2

   NOTE: this subcommand is optional for DMOVE to function

  
<span id="matrix"></span> **matrix**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | total matrix gain | relative multiplier of input signal | no | no | 
p1-p145 | 12 x 12 matrix amp/feedback coefficients |  -  | no | yes | defaults to internal matrix | 

   NOTE: this subcommand is optional for DMOVE to function

  

-----

  
**DMOVE** is a [pfield-enabled](pfield-enabled.html) version of the
[MMOVE](MMOVE.html) room-simulation program, allowing the sound source
trajectory to be controlled using
[maketable](../scorefile/maketable.html) tables or dynamically via the
[makeconnection](../scorefile/makeconnection.html) scorefile command. It
uses the same methodology as the [MPLACE](MPLACE.html) instrument to
model the acoustics of a room.

### Usage Notes

Most of the subcommands for **DMOVE** are identical to the equivalent
subcommands in [MPLACE](MPLACE.html). See the [MPLACE Usage
Notes](MPLACE.html#usage_notes) for more information.

The only semi-tricky thing to know about **DMOVE** is that the
interpretation of the pfield data coming through p4 and p5 ("DIST-XPOS",
"ANGLE-YPOS") depends on whether or not p6 ("dist\_mikes") is positive
or negative. The distance to the mikes (p6) will be interpreted as a
positive number in any case, but if it is negative it will cause p4 and
p5 to be interpreted as cartesian coordinates on a coordinate plane
where the center of the listener is located at \[0, 0\]. The default
interpretation is to use polar coordinates for the sound source
information (distance to sound, angle to sound).

When using [maketable](../scorefile/maketable.html) and other
pfield-control parameters be aware that the "nonorm" optional parameter
should probably be set, otherwise the data coming through the pfield
will be normalized between 0.0 and 1.0 (or -1.0 and 1.0).

The **RVB** subinstrument needs to be configured with the appropriate
[bus\_config](../scorefile/bus_config.html) setup. **DMOVE/RVB**
requires stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2, 256)
   load("DMOVE")
   
   bus_config("DMOVE", "in 0", "aux 0-1 out")
   bus_config("RVB", "aux 0-1 in", "out 0-1")

   rtinput("mysoundfile.aif")
   
   mikes(45, 0.5)
   
   dist_front = 100
   dist_right = 100
   dist_rear = -100
   dist_left = -100
   height = 100
   rvbtime = 3
   abs_fac = 1
   space(dist_front, dist_right, dist_rear, dist_left, height, abs_fac, rvbtime)
   
   
   insk = 0
   outsk = 0
   amp = 1
   
   dur = 60
   dist_mikes = 2
   inchan = 0
   
   xpos = makeconnection("mouse", "x", min=dist_left, max=dist_right, dflt=10, lag=90,
      "x pos", "feet", 2)
   
   ypos = makeconnection("mouse", "y", min=dist_rear, max=dist_front, dflt=10, lag=90, 
      "y pos", "feet", 2)
   
   mindist = 10
   maxdist = 100
   
   set_attenuation_params(mindist, maxdist, 1.0)
   threshold(0.0005)
   
   DMOVE(outsk, insk, dur, amp, xpos, ypos, -dist_mikes, inchan)
   
   RVB(0, 0, dur+rvbtime+0.5, 0.1)
```

  

-----

### See Also

[FREEVERB](FREEVERB.html), [GVERB](GVERB.html), [MMOVE](MMOVE.html),
[MOVE](MOVE.html), [MPLACE](MPLACE.html), [MROOM](MROOM.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
