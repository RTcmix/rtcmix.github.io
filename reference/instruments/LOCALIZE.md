---
title: LOCALIZE()
layout: ref
---

## LOCALIZE

Delay/amplitude/filter-based localization instrument.

*in RTcmix/insts/bgg*  
  

-----

##### quick syntax:

**LOCALIZE**(outsk, insk, dur, AMP, X-SRC, Y-SRC, Z-SRC, X-DEST, Y-DEST,
Z-DEST, HEADWIDTH, feet/unit\[, inchan, headfilt, ampdisttype, minamp,
maxdist)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output skip time |  -  | no | no | 
p1 | input skip time |  -  | no | no | 
p2 | duration |  seconds  | no | no | 
p3 | overall amp |  -  | yes | no | 
p4 | source x | see Usage Notes | yes | no | 
p5 | source y | see Usage Notes | yes | no | 
p6 | source z | see Usage Notes | yes | no | 
p7 | dest (listener) x |  see Usage Notes | yes | no |  | 
p8 | dest (listener) y |  see Usage Notes | yes | no |  | 
p9 | dest (listener) z |  see Usage Notes | yes | no |  | 
p10 | headwidth | see Usage Notes | yes | no | 
p11 | feet/unit scaler |  -  | no | no | 
p12 | input channel |  -  | no | no | default: 0
p13 | behind head filter on/off | no | no | default: 0 (off) |
p14 | amp/distance calculation flag | 0: no amp/distance, 1: linear amp/distance, 2: inverse square amp/distance | no | no | default: 0 |
p15 | minimum amp/distance multiplier |  -  | no | no | default: 0 
p16 | maximum distance (for linear amp/distance scaling) | see Usage Notes | no | no | default: 0 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

The speed of sound set at 1000 ft/second

Author: Brad Garton (garton - at - columbia.edu), 11/2017

  

-----

  
**LOCALIZE** calculates distance and angle delays and amplitudes for 3-D
sound localization in virtual environments. A crude 'behind-the-head'
filter is also included.

### Usage Notes

This instrument is designed for use in RTcmix/Unity to localize
GameObject sound sources. The 'source' x/y/z coordinates are usually the
coordinates of the GameObject making the sound *relative* to the
AudioListener receiving the sound (which means that p7/p8/p9 are usually
set to 0.0). These relative coordinates can be ascertained using the
*getMyTransform.cs* script as a component on the AudioListener (see
[RTcmix/Unity
demos](https://sites.music.columbia.edu/cmc/courses/g6610/fall2017/syl.html)
for examples of this). This approach allows the instrument to take into
account the 3-D rotation of the AudioListener as well as the location in
3-space.

One non-Unity thing: the x/y plane in LOCALIZE is the lateral plane,
with the z axis going up and down. In Unity the x/z plane is the lateral
plane with the y axis up/down. The y and z coordinates thus need to be
flipped when using LOCALIZE in Unity; i.e. put the z-coordinate in place
of the y-coordinate (and vice-versa). It's not clear why this is the
convention in Unity, but LOCALIZE uses the x/y plane as the primary
lateral plane

The 'head filter' amp rolloff when the source is to the left or right of
the listener is a simple 60% decrease when the source is 90 degrees
right or left. This was done 'by ear'. Also, the behind-the-head filter
is only a basic low-pass filter at present. Both of these factors
increase as the sound source moves to the right/left or behind the
listener. Plans are to incorporate a more robust HRTF filter in the
future.

p10 (the "headwidth") is specified in terms of the virtual environment
units. p11 ("feet/unit") acts as a unit-to-feet scaler, as the
distance/amp and delay calculations in LOCALIZE are based on sound
traveling 1000 feet/second. A value of 10.0 would specify that one
virtual environment unit is equivalent to 10 feet in the 'real world'.

p13 ("headfilt") turns on or off (0 or 1) the low-pass behind-the-head
filter.

p14 ("ampdisttype") determines how the amplitude/distance roll-off is
calculated. "0" specifies no roll-off, "1" uses a linear roll-off, and
"2" uses the inverse square law (amp roll-off is propotional to
1/distance), power is 1/distance-squared).

If p14 is set to a value of "1" or "2". p15 ("minamp") and p16
"maxdist") become operational. p15 sets a minimum amplitude. Sound
sources will not go below this amplitude no matter how far they are from
the listener. p16 is used to set the maximum distance for the linear amp
roll-off when p14 is 1.

LOCALIZE does not do any up/down lateralization (z axis) at present,
although the z coordinate is used in the distance calculations.

LOCALIZE requires a stereo output.

### Sample Score

very basic:

```cpp
   rtsetparams(44100, 2)
   load("LOCALIZE")

   reset(44100)

   rtinput("mysound.aif")

   amp = maketable("line", 1000, 0,0, 1,1, 2,0)

    // circle around the listener on the x/y plane
   srcX = maketable("line", "nonorm", 1000, 0,-4, 1,4, 2,-4)
   srcY = maketable("line", "nonorm", 1000, 0,0, 1,4, 2,0, 3,-4, 4,0)

   LOCALIZE(0, 0, 9, 1,  srcX, srcY, 0.0, 0.0, 0.0, 0.0,  0.1, 10, 1)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[bus\_config](../scorefile/bus_config.html), [PAN](PAN.html),
[DMOVE](DMOVE.html), [MPLACE](MPLACE.html), [MMOVE](MMOVE.html),
[PLACE](PLACE.html), [MOVE](MOVE.html), [SROOM](SROOM.html),
[MROOM](MROOM.html), [ROOM](ROOM.html), [FREEVERBK](FREEVERB.html),
[GVERB](GVERB.html), [REV](REV.html), [REVERB](REVERBIT.html)
