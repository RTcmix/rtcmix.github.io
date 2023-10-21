---
title: MMOVE()
layout: ref
---

## MMOVE

Moving source room-simulation.

*in RTcmix/insts/std/MMOVE*  
  

-----

##### quick syntax:

**MMOVE**(outskip, inskip, dur, AMP, dist\_between\_mikes\[,
inputchan\])  
  
**RVB**(outsk, insk, dur, AMP)  
  
**space**(front, right, -back, -left, ceiling, absorb, rvbtime)  
  
**path**(time0, sound\_dist0, sound\_angle0, ... timeN, sound\_distN,
sound\_angleN)  
  
**cpath**(time0, sound\_x-coord0, sound\_y-coord0, ... timeN,
sound\_x-coordN, sound\_y-coordN)  
  
**param**(function1, function2)  
  
**cparam**(function1, function2)  
  
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

  
**MMOVE** employs several subcommands to set the room-simulation
characteristics, the sound-source trajectory and one sub-instrument
(**RVB**) to operate.

NOTE: This is an older RTcmix instrument, the newer [DMOVE](DMOVE.html)
instrument allows the sound trajectory to be controlled using
[pfield-enabled](pfield-enabled.html) parameters.  
  
  
<span id="MMOVE"></span> **MMOVE**  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = distance between 'mics' (stereo receivers) in the room (feet)
   p5 = input channel [optional, default is 0]

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.
```

  
<span id="RVB"></span> **RVB**  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.

   NOTE: this associated instrument is required for MMOVE to function
```

  
<span id="space"></span> **space**  

```cpp
   p0 = distance to front wall of room (feet)
   p1 = distance to right-hand wall of room (feet)
   p2 = distance to back wall of room (feet) [< 0.0]
   p3 = distance to left-hand wall of room (feet) [< 0.0]
   p4 = distance to ceiling of room (feet)
   p5 = wall absorption factor (0-10; 0 == more 'dead', 10 == more 'live')
   p6 = reverberation time (seconds)

   NOTE: this subcommand is required for MMOVE to function
```

  
<span id="path"></span> **path**  

```cpp
   The pfields for path are triples, the first being the relative time
   during processing to reach this point, and the other two of each triple being
   polar coordinates of the sound source location (distance to sound [feet]
   and angle to sound [degrees]).  Up to 100 triples may be specified.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MMOVE to function
```

  
<span id="cpath"></span> **cpath**  

```cpp
   The pfields for cpath are triples, the first being the relative time
   during processing to reach this point, and the other two of each triple being
   the x- and y- cartesian coordinates of the sound source location (feet,
   with [0,0] being the center position of the listener).  Up to 100 triples
   may be specified.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MMOVE to function
```

  
<span id="param"></span> **param**  

```cpp
   p0 = function table reference for polar coordinate distance to sound source (feet) values
   p1 = function table reference for polar coordinate angle to sound source (degrees) values

   The two function tables are loaded with values representing the polar
   coordinates of the sound source location (p0 table == distance to sound [feet]
   and p1 table == angle to sound [degrees]).  These values will be spread
   over the duration of the note

   Because this instrument has not been updated for pfield control, the older
   makegen function table system should be used to create the tables.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MMOVE to function
```

  
<span id="cparam"></span> **cparam**  

```cpp
   p0 = function table reference for x-coordinate location of sound source (feet)
   p1 = function table reference for y-coordinate location of sound source (feet)

   The two function tables are loaded with values representing the x-coordinate
   location of the sound source (feet) and the y-coordinate location of the
   sound source (feet).  The listener is assumed to be centered at coordinate [0,0].
   These values will be spread over the duration of the note

   Because this instrument has not been updated for pfield control, the older
   makegen function table system should be used to create the tables.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MMOVE to function
```

  
<span id="threshold"></span> **threshold**  

```cpp
   p0 = time interval (seconds) for trajectory updates (typically < 0.01)

   NOTE: this subcommand is optional for MMOVE to function (the default is
      the size of the buffers set in rtsetparams)
```

  
<span id="mikes"></span> **mikes**  

```cpp
   p0 = microphone angle (degrees, 0 degrees is straight in front)
   p1 = microphone pattern (0-1; 0 == omnidirectional, 1 == highly directional)

   NOTE: this subcommand is optional for MMOVE to function (the default is "mikes_off")
```

  
<span id="mikes_off"></span> **mikes\_off**  

```cpp
   no pfields, this defeats the microphone angle and pattern settings for binaural simulation

   NOTE: this subcommand is optional for MMOVE to function
```

  
<span id="set_attenuation_params"></span> **set\_attenuation\_params**  

```cpp
   p0 = minimum distance (feet)
   p1 = maximum distance (feet)
   p2 = distance attentuation exponent

   NOTE: this subcommand is optional for MMOVE to function
```

  
<span id="matrix"></span> **matrix**  

```cpp
   p0 = total matrix gain (relative multiplier of input signal)
   p1-p145 = 12 x 12 matrix amp/feedback coefficients [optional; defaults to internal matrix]

   NOTE: this subcommand is optional for MMOVE to function
```

  

-----

  
**MMOVE** is an updated version of the original [MOVE](MOVE.html)
room-simulation program. It uses the same methodology as the
[MPLACE](MPLACE.html) instrument to model the acoustics of a room. The
difference is that **MMOVE** allows you to specify a trajectory for the
sound source instead of a single, fixed location for the duration of
processing.

NOTE: This is an older RTcmix instrument, the newer [DMOVE](DMOVE.html)
instrument allows the sound trajectory to be controlled using
[pfield-enabled](pfield-enabled.html) parameters.
<span id="usage_notes"></span>

### Usage Notes

Most of the subcommands for **MMOVE** are identical to the equivalent
subcommands in [MPLACE](MPLACE.html). See the [MPLACE Usage
Notes](MPLACE.html#usage_notes) for more information.

The new subcommands in **MMOVE** (**path**, **cpath**, **param**,
**cparam**) relate to specifying the sound source trajectory over the
duration of processing. The parameters should be self-explanatory; you
set the coordinate locations for the sound source (in polar or cartesian
coordinates) at relative times and the instrument will calculate the
movement trajectory to intersect these points. For higher rates of speed
the delay-line interpolation used in **MMOVE** will effectively simulate
a Doppler shift in all the calculated sound paths. Be careful, it is
easy to inadvertently create high rates of speed for the sound source\!

The **threshold** rate will determine how often the various delay
lengths and attentuation factors are calculated, based on where the
sound source location has moved along the trajectory. This rate is
independent of the [reset](../scorefile/reset.html) rate used for pfield
updates.

The **RVB** subinstrument needs to be configured with the appropriate
[bus\_config](../scorefile/bus_config.html) setup. **MPLACE/RVB**
requires stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2, 1024)
   load("MMOVE")

   bus_config("MMOVE", "in 0", "aux 0-1 out")
   bus_config("RVB", "aux 0-1 in", "out 0-1")
   
   rtinput("mysoundfile.wav")

   mikes(45, 0.5)
   
   dist_front = 100
   dist_right = 130
   dist_rear = -145
   dist_left = -178
   height = 100
   rvbtime = 2
   abs_fac = 4
   space(dist_front, dist_right, dist_rear, dist_left, height, abs_fac, rvbtime)
   
   /* 5 feet is min distance,  100 feet max,  scale by 1/distance**1.5 */
   set_attenuation_params(5.0, 100.0, 1.5);
   
   insk = 0
   outsk = 0
   dur = 15;
   pre_amp = 25
   dist_mikes = 2.2     /* for normal */
   //dist_mikes = 0.67  /* for binaural */
   inchan = 0
   
   threshold(0.0005)
   
   // slow,  then zoom in and out again
   path(0,50,-90, 10,50,0, 11,10,20, 12,80,30, 15,30,90)
   
   // MMOVE does not have rvb level arg that MOVE had.  Handled in RVB call now
   MMOVE(outsk, insk, dur, pre_amp, dist_mikes, inchan)
   
   // Reverb gain increases over time
   handle rvblevel
   rvblevel = maketable("line", 1024, 0, 0.01, 1, 1.0);
   RVB(0, 0, dur+rvbtime+0.5, rvblevel)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MOVE](MOVE.html), [MPLACE](MPLACE.html), [MROOM](MROOM.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
