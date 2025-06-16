---
title: QMOVE()
layout: ref
---

## QMOVE

Quadraphonic pfield-enabled moving source room-simulation.

*in RTcmix/insts/std/MMOVE*  
  

-----

##### quick syntax:

**QMOVE**(outsk, insk, dur, AMP, DIST-XPOS, ANGLE-YPOS,
(-)dist\_to_mikes\[, inchan\]);  
  
**QRVB**(outsk, insk, dur, AMP)  
  
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

  
**QMOVE** employs several subcommands to set the room-simulation
characteristics and one sub-instrument (**QRVB**) to operate.  The subcommands are described on the [DMOVE](DMOVE.html) page.

For all commands below: Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

  
<span id="QMOVE"></span> **QMOVE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration (or endtime if negative) | seconds | no | no |
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | distance to, or x-coordinate of, sound source | feet* | yes | no | * p6 < 0: cartesian, else polar | 
p5 | angle to, or y-coordinate of, sound source | feet* or degrees (0 is straight in front) | yes | no | * p6 < 0: cartesian, else polar
p6 | distance from listener to microphones | feet | no | no | if negative, p4/p5 will be interpreted as x- and y- coordinates, otherwise p4/p5 will set polar coordinates
p7 | input channel |  -  | no | yes | default: 0 | 

  
<span id="QRVB"></span> **QRVB**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration (or endtime if negative) | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 

   NOTE: this associated instrument is required for **QMOVE** to function
  

-----

  
**QMOVE** is a 4-channel version of the
[DMOVE](DMOVE.html) room-simulation program, allowing the sound source
trajectory to be controlled using
[maketable](../scorefile/maketable.html) tables or dynamically via the
[makeconnection](../scorefile/makeconnection.html) scorefile command.

### Usage Notes

Most of the subcommands for **QMOVE** are identical to the equivalent
subcommands in [MPLACE](MPLACE.html). See the [MPLACE Usage
Notes](MPLACE.html#usage_notes) for more information.

Note that the **dist\_to\_mikes** parameter is different than for all other versions of this program.  Rather than the distance between microphones, here it represents the distance **to each** of the four microphones, which will be virtually arrayed in a square with the listener in the center.

One tricky thing to know about **QMOVE** is that the
interpretation of the pfield data coming through p4 and p5 ("DIST-XPOS",
"ANGLE-YPOS") depends on whether or not p6 ("dist\_to_mikes") is positive
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

The **QRVB** subinstrument needs to be configured with the appropriate
[bus\_config](../scorefile/bus_config.html) setup. **QMOVE/QRVB**
requires 4-channel output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 4, 256)
   load("QMOVE")
   
   bus_config("QMOVE", "in 0", "aux 0-3 out")
   bus_config("QRVB", "aux 0-3 in", "out 0-3")

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
   dist_to_mikes = 8;  // each mike is 8 feet away at the corners of a square
   inchan = 0
   
   xpos = makeconnection("mouse", "x", min=dist_left, max=dist_right, dflt=10, lag=90,
      "x pos", "feet", 2)
   
   ypos = makeconnection("mouse", "y", min=dist_rear, max=dist_front, dflt=10, lag=90, 
      "y pos", "feet", 2)
   
   mindist = 10
   maxdist = 100
   
   set_attenuation_params(mindist, maxdist, 1.0)
   threshold(0.0005)
   
   QMOVE(outsk, insk, dur, amp, xpos, ypos, -dist_to_mikes, inchan)
   
   QRVB(0, 0, dur+rvbtime+0.5, 0.1)
```

  

-----

### See Also

[FREEVERB](FREEVERB.html), [GVERB](GVERB.html), [DMOVE](DMOVE.html),
[MMOVE](MMOVE.html), [MPLACE](MPLACE.html), [MROOM](MROOM.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
