---
title: NPAN()
layout: ref
---

## NPAN

Multichannel pair-wise intensity panner.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**NPANspeakers**(mode, speak1\_a, speak1\_b ... speakN\_a, speakN\_b)  
  
**NPAN**(outsk, insk, dur, AMP, mode, ANGLE/XLOC, DISTANCE/YLOC\[,
inputchan\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**NPAN** consists of two sub-instruments, one to set up the 'speaker'
placements (**NPANspeakers**) and one to process the sound source
through the modeled placements (**NPAN**).  
  
  
<span id="NPANspeakers"></span> **NPANspeakers**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | mode | "polar" or "xy" (or "cartesian")) | no | no | 
p1 | virtual speaker 1 position | angle or x-location | no | no | determined by mode
p2 | virtual speaker 1 location | distance or y-location | no | no | determined by mode
...
pN-1 | virtual speaker N/2 position | angle or x-location | no | no | determined by mode
pN   | virtual speaker N/2 location | distance or y-location | no | no | determined by mode

      pfields p1,p2, p3,p4, pN-1,pN are pairs specifying the locations
      of the virtual speakers, using angle/distance coordinates (for "polar"
      mode) or x-location/y-location (for "xy" or "cartesian" mode).  Distances
      are assumed to be in feet.  Up to 16 speakers (32 pairs) may be set.

  
<span id="NPAN"></span> **NPAN**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | seconds | no | no | 
p3 | global amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | mode: "polar" or "xy" | or "cartesian" | no | no | 
p5 | angle | degrees relative to listener | yes | no | If mode is "polar"
   |  x coordinate | feet | yes | no | If mode is "cartesian"
p6 | distance from listener | feet | no | no | If mode is "polar"
   | y coordinate | feet | yes | no | If mode is "cartesian"
p7 | input channel |  -  | no | yes | default: 0 | 

   Author:  John Gibson, 11/13/04
   based on the description in F. R. Moore, "Elements of Computer Music," pp. 353-9

  

-----

  
**NPAN** is very useful for multi-speaker 'spatialization' applications.

### Usage Notes

To use **NPAN**, first call **NPANspeakers** to configure the number of
speakers and their locations.

In **NPANspeakers**, If you choose "polar" mode for p0 ("mode"), the
listener is situated in the center of the coordinate space, at \[0,0\].
Angles are measured in degrees, with 0 degrees directly in front of the
listener, 90 degrees to their left and -90 degrees to their right.

"cartesian" or "xy" mode for p0 will set up a Cartesian coordinate plane
with the listener again at \[0,0\]. Speaker locations are given in terms
of their x-coordinates and y-coordinates. Distance in both "polar" mode
and "xy" mode are in feet.

The order of speakers in the argument list for **NPANspeakers** is
significant: the order corresponds to the order of output channels. So
if you want the front left speaker in the first output channel, list it
first.

Then using **NPAN**, parameters p5 ("ANGLE/XLOC,") and p6
("DISTANCE/YLOC") give the position of the virtual sound source,
relative to the listener, using the coordinate space described above. As
with the speaker setup, the interpretation of p5 and p6 depends on the
"mode" parameter (p4).

This is pair-wise panning, so no more than two adjacent speakers have
signal at any time. It is not possible to place a virtual source between
two speakers that are not adjacent.

Distance from the listener affects gain (the closer, the higher the
gain). Very short distances can cause clipping. If in doubt, use the
polar mode, set both speaker distances and virtual source distance to 1.

A multichannel interface card is needed for this command, note the
[rtsetparams](../scorefile/rtsetparams.html) commands in the sample
scores all specify multiple channel outputs.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 4)
   load("WAVETABLE")
   load("NPAN")

   bus_config("WAVETABLE", "aux 0 out")
   bus_config("NPAN", "aux 0 in", "out 0-3")

   dur = 10
   amp = 10000
   freq = 440

   env = maketable("line", 1000, 0,0, 1,1, 19,1, 20,0)
   WAVETABLE(0, dur, amp*env, freq, 0)

   NPANspeakers("polar",
       45, 1,     // left front
      -45, 1,     // right front
       135, 1,    // left rear
      -135, 1)    // right rear

   dist = 1

   // 3 counter-clockwise trips around circle
   trips = 3
   angle = maketable("linebrk", "nonorm", 1000, 45, 1000, 405 * trips)

   NPAN(0, 0, dur, 1, "polar", angle, dist)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 8)
   load("NPAN")

   rtinput("mysound.snd")

   // 8 speakers arranged in circle, with speakers directly in front of (0 deg)
   // and behind (180 deg) listener.

   NPANspeakers("polar",
       45, 1,   // front left
      -45, 1,   // front right
       90, 1,   // side left
      -90, 1,   // side right
      135, 1,   // rear left
     -135, 1,   // rear right rear
        0, 1,   // front center
      180, 1)   // rear center

   inskip = 0
   amp = 1.0
   dur = DUR()
   start = 0

   // move counter-clockwise around circle
   angle = maketable("line", "nonorm", 1000, 0,0, 1,360)

   dist = maketable("line", "nonorm", 1000, 0,.5, 1,4, 6,.4)

   NPAN(start, inskip, dur, amp, "polar", angle, dist)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 8, 512)
   load("WAVETABLE")
   load("NPAN")

   bus_config("WAVETABLE", "aux 0 out")
   bus_config("NPAN", "aux 0 in", "out 0-7")

   // 8 speakers arranged in a rectangle, as follows...
   //
   //    1  7  2
   //    3  x  4        x = listener
   //    5  8  6

   sin45 = 0.70710678

   NPANspeakers("xy",
      -1,     1,     // front left (1)
       1,     1,     // front right (2)
      -sin45, 0,     // side left (3)
       sin45, 0,     // side right (4)
      -1,    -1,     // rear left (5)
       1,    -1,     // rear right rear (6)
       0, sin45,     // front center (7)
       0, -sin45)    // rear center (8)

   dur = 60
   amp = 10000
   freq = 440

   env = maketable("line", 1000, 0,0, 1,1, 49,1, 50,0)
   WAVETABLE(0, dur, amp*env, freq, 0)

   // pan using the mouse
   lag = 70
   x = makeconnection("mouse", "X", -1, 1, 0, lag, "X")
   y = makeconnection("mouse", "Y", -1, 1, 1, lag, "Y")

   NPAN(0, 0, dur, 1, "xy", x, y)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconnection](../scorefile/makeconnection.html), [DMOVE](DMOVE.html),
[MIX](MIX.html), [MMOVE](MMOVE.html), [MOVE](MOVE.html),
[MPLACE](MPLACE.html), [MROOM](MROOM.html), [PAN](PAN.html),
[PLACE](PLACE.html), [QPAN](QPAN.html), [ROOM](ROOM.html),
[SROOM](SROOM.html) [STEREO](STEREO.html)
