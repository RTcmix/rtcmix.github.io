---
title: MOVE()
layout: ref
---

## MOVE

Moving-source room simulation.

*in RTcmix/insts/std/MOVE*  
  

-----

##### quick syntax:

**MOVE**(outskip, inskip, dur, AMP, dist\_between\_mikes, rvb\_amp\[,
inputchan\])  
  
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

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**MOVE** employs several subcommands to set the room-simulation
characteristics and the sound-source trajectory

NOTE: This is an older RTcmix instrument, the newer [MMOVE](MMOVE.html)
or [DMOVE](DMOVE.html) (pfield-enabled data specification) instruments
are probably better to use.  
  
  
<span id="MOVE"></span> **MOVE**  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = distance between 'mics' (stereo receivers) in the room (feet)
   p5 = input channel [optional, default is 0]

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.
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

   NOTE: this subcommand is required for MOVE to function
```

  
<span id="path"></span> **path**  

```cpp
   The pfields for path are triples, the first being the relative time
   during processing to reach this point, and the other two of each triple being
   polar coordinates of the sound source location (distance to sound [feet]
   and angle to sound [degrees]).  Up to 100 triples may be specified.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MOVE to function
```

  
<span id="cpath"></span> **cpath**  

```cpp
   The pfields for cpath are triples, the first being the relative time
   during processing to reach this point, and the other two of each triple being
   the x- and y- cartesian coordinates of the sound source location (feet,
   with [0,0] being the center position of the listener).  Up to 100 triples
   may be specified.

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MOVE to function
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

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MOVE to function
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

   NOTE: one of the subcommands (path, cpath, param, cparam) is required for MOVE to function
```

  
<span id="threshold"></span> **threshold**  

```cpp
   p0 = time interval (seconds) for trajectory updates (typically < 0.01)

   NOTE: this subcommand is optional for MOVE to function (the default is
      the size of the buffers set in rtsetparams)
```

  
<span id="mikes"></span> **mikes**  

```cpp
   p0 = microphone angle (degrees, 0 degrees is straight in front)
   p1 = microphone pattern (0-1; 0 == omnidirectional, 1 == highly directional)

   NOTE: this subcommand is optional for MOVE to function (the default is "mikes_off")
```

  
<span id="mikes_off"></span> **mikes\_off**  

```cpp
   no pfields, this defeats the microphone angle and pattern settings for binaural simulation

   NOTE: this subcommand is optional for MOVE to function
```

  
<span id="matrix"></span> **matrix**  

```cpp
   p0 = total matrix gain (relative multiplier of input signal)
   p1-p145 = 12 x 12 matrix amp/feedback coefficients [optional; defaults to internal matrix]

   NOTE: this subcommand is optional for MOVE to function
```

  

-----

  
**MOVE** is Doug Scott's moving sound simulation instrument, which
simulates the acoustic space of a room and lets the user define the
trajectory of a sound moving through space within the room, relative to
the listener's position. This is similar to Doug's other room-simulation
program, [PLACE](PLACE.html), the difference being the ability to
specify a moving sound source. **MOVE** is a bit more computationally
expensive than PLACE. The instrument has optional parameters for
microphone placement and the computation of inter-aural delays.

NOTE: This is an older RTcmix instrument, the newer [MMOVE](MMOVE.html)
or [DMOVE](DMOVE.html) (pfield-enabled data specification) instruments
are probably better to use. <span id="usage_notes"></span>

### Usage Notes

The use of **MOVE** is almost identical to the newer [MMOVE](MMOVE.html)
instrument. See the [MMOVE Usage Notes](MMOVE.html#usage_notes) for that
instrument (minus the updated subcommands, of course). In fact, it's
probably better to use either [MMOVE](MMOVE.html) or [DMOVE](DMOVE.html)
for doing this processing.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2)
   load("MOVE")

   rtinput("mysound.snd")
   
   space(50, 50, -750, -80, 25, 1, 3)
   mikes_off()
   path(0, 25, 0, 3, 15, 90)
   
   MOVE(0, 0, 5, 20, 0, 1)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MPLACE](MPLACE.html), [MROOM](MROOM.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
