---
title: MPLACE()
layout: ref
---

## MPLACE

Stationary source room-simulation.

*in RTcmix/insts/std/MMOVE*  
  

-----

##### quick syntax:

**MPLACE**(outskip, inskip, dur, AMP, dist-xpos, angle-ypos,
(-)dist\_mikes\[, input\_channel\])  
  
**RVB**(outskip, inskip, dur, AMP)  
  
**space**(front, right, -back, -left, ceiling, absorb, rvbtime)  
  
**mikes**(mike\_angle, pattern)  
  
**mikes\_off**()  
  
**set\_attenuation\_params**(min\_dist, max\_dist, dist\_exponent)  
  
**matrix**(amp, matrixval1, matrixval2, ... matrixval144)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**MPLACE** employs several subcommands to set the room-simulation
characteristics and one sub-instrument (**RVB**) to operate.  
  
  
<span id="MPLACE"></span> **MPLACE**  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = distance (feet) to sound source, or x-coordinate (feet) of sound source
   p5 = angle to sound source (degrees; 0 degrees is straight in front),
      or y-coordinate (feet) of sound source
   p6 = distance between 'mics' (stereo receivers) in the room (feet)
      NOTE: if p6 is negative, p4/p5 will be interpreted as x- and y- coordinates,
      otherwise p4/p5 will set polar coordinates for the sound source location
   p7 = input channel [optional, default is 0]

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.
```

  
<span id="RVB"></span> **RVB**  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds) 
   p3 = amplitude multiplier (relative multiplier of input signal)

   p3 (amplitude) can receive dynamic updates from a table or real-time control source.

   NOTE: this associated instrument is required for MPLACE to function
```

  
<span id="space"></span> **space**  

``` 
   p0 = distance to front wall of room (feet)
   p1 = distance to right-hand wall of room (feet)
   p2 = distance to back wall of room (feet) [< 0.0]
   p3 = distance to left-hand wall of room (feet) [< 0.0]
   p4 = distance to ceiling of room (feet)
   p5 = wall absorption factor (0-10; 0 == more 'dead', 10 == more 'live')
   p6 = reverberation time (seconds)

   NOTE: this subcommand is required for MPLACE to function
```

  
<span id="mikes"></span> **mikes**  

``` 
   p0 = microphone angle (degrees, 0 degrees is straight in front)
   p1 = microphone pattern (0-1; 0 == omnidirectional, 1 == highly directional)

   NOTE: this subcommand is optional for MPLACE to function (the default is "mikes_off")
```

  
<span id="mikes_off"></span> **mikes\_off**  

``` 
   no pfields, this defeats the microphone angle and pattern settings for binaural simulation

   NOTE: this subcommand is optional for MPLACE to function
```

  
<span id="set_attenuation_params"></span> **set\_attenuation\_params**  

``` 
   p0 = minimum distance (feet)
   p1 = maximum distance (feet)
   p2 = distance attentuation exponent

   NOTE: this subcommand is optional for MPLACE to function
```

  
<span id="matrix"></span> **matrix**  

``` 
   p0 = total matrix gain (relative multiplier of input signal)
   p1-p145 = 12 x 12 matrix amp/feedback coefficients [optional; defaults to internal matrix]

   NOTE: this subcommand is optional for MPLACE to function
```

  

-----

  
**MPLACE** is an updated version of [PLACE](PLACE.html), Doug Scott's
room simulation RTcmix instrument. It places a sound in a room of your
design and simulates the acoustic resonances of the room. Early
reflections are computed using a ray-tracing approach based in the
distances between the source, the listening 'microphones' and the walls.
The generalized room reverberation is simulated using a 12 x 12 matrix
of delay elements with feedback and filtering designed to imitate the
global reverberation response of the specified room. This is done using
the associated **RVB** instrument. Localization of the source sound is
very good using **MPLACE**. <span id="usage_notes"></span>

### Usage Notes

The room design for **MPLACE** depends on parameters specified in the
**setup** subcommand. The parameters work by assuming that the listener
(YOU) is placed at point \[0,0\] in a coordinate space. The locations of
the four walls are specified in this x,y coordinate system ("front",
"right", "-back", "-left"):

``` 
                                  y
                                  |
                             -x---0---x
                                  |
                                 -y
```

Therefore the distance to the back and left walls must be specified as
negative. All, including height, are in feet. "absorb" is a number
between 0 (minimum) and 10 (maximum) that determines how reflective the
walls are. "rvbtime" is the approximate length of reverb in seconds (the
actual length depends on the size of the room).

**MPLACE** sets the location of the source sound by specifying the
distance to the sound ("dist-xpos") and the angle ("angle\_ypos") of the
sound:

``` 

                                  0.0    source
                                   |    /
                                   |   /
                                   |  /
                                   | /
                                   |/
                                  you
```

using feet and degrees, respectively (see below for an alternative
method of specifying the source location). Be sure to stay inside bounds
of the room (it may take a bit of trigonometry \[or guessing\] to do
that) The "AMP" pfield is a multipler of the input audio prior to any
processing by **MPLACE**. The "dist\_mikes" parameter sets how far apart
the two 'listening' points are from the \[0,0\] center of the coordinate
room (you can choose to think of this is how big the listener's head
is...).

A call to **MPLACE** should be followed by a call to **RVB** to create
the diffuse room reverberation. **RVB** should be configured using the
[bus\_config](../scorefile/bus_config.html) aux bus system to received
the audio output from **MPLACE**.

When specifying the parameters for **MPLACE**, be aware that the
interpretation of the p4 and p5 pfields ("dist-xpos", "angle-ypos")
depends on whether or not p6 ("dist\_mikes") is positive or negative.
The distance to the mikes (p6) will be interpreted as a positive number
in any case, but if it is entered as negative it will cause p4 and p5 to
be interpreted as cartesian coordinates on a coordinate plane where the
center of the listener is located at \[0, 0\]. The default
interpretation is to use polar coordinates for the sound source
information (distance to sound, angle to sound).

The optional subcommands that may be used to set the parameters for
**MPLACE** include **mikes** and **mikes\_off**. **mikes** is a setup
call for times when you wish to simulate microphone recording (most of
the time, actually). The arguments are "mike\_angle", which is the angle
*each* microphone makes with a line drawn between them (i.e., 45 puts
them at a 90 degree angle FROM EACH OTHER). "pattern" is a value between
0 and one; 0 produces an omnidirectional mike, 1 produces a
hypercardioid pattern, 0.5 produces the familiar cardioid pattern. The
default pattern will be omnidirectional.

**mikes\_off** is used following a previous call to **mikes** in order
to cancel mike mode in the event that you wish to use binaural mode, or
default to omnidirectional.

**set\_attenuation\_params** can set certain values that may be
important during processing. p0 sets a minimum distance for a sound in
feet. If the sound approaches the mike or ear closer than this, its
volume will not increase beyond the maximum level (unity gain). Default
is 0.0.

p1 sets the maximum distance for a sound in feet. If the sound exceeds
this distance, its volume is not attenuated beyond the level associated
with the maximum distance. Default is infinite.

The attentation factor (p2) determines the amount sound will decay over
the distance traveled. Sounds will attenuate according to the formula:

``` 
   a = 1.0 / ((d/mindistance) ** (factor))
```

where *d* is the distance to the sound, and *factor* is the attenuation
factor (2.0 is the default value). NOTE: In the current version, ALL
sound path gains are affected by these values, including all first- and
second-generation reflections\! Modifying these values will therefore
produce significant and interesting changes in the reverb as well as the
gain of the sounds.

**matrix** is used to replace the default set of amplitude/feedback
coefficients for the **RVB** matrix -- all 144 of them. This is tricky
to use.

Note that multiple **MPLACE** notes can feed into a single **RVB**
instrument for more efficient processing.

**MPLACE/RVB** requires stereo output.

### Sample Scores

basic use:

``` 
   rtsetparams(44100,2,4096)
   load("MPLACE")

   bus_config("MPLACE", "in 0", "aux 0-1 out")
   bus_config("RVB", "aux 0-1 in", "out 0-1")

   rtinput("mysoundfile.wav")
   
   mikes(45, 0.5)
   
   dist_front = 100
   dist_right = 130
   dist_rear = -145
   dist_left = -178
   height = 100
   rvbtime = 3
   abs_fac = 8
   space(dist_front, dist_right, dist_rear, dist_left, height, abs_fac, rvbtime)
   
   insk = 0
   outsk = 0
   dur = DUR(0)
   pre_amp = 120
   dist2sound = 60
   angle_sound = 90
   dist_mikes = 5
   //dist_mikes = 0.67  /* binaural */
   inchan = 0
   
   MPLACE(outsk, insk, dur, pre_amp, dist2sound, angle_sound+180, dist_mikes, inchan)
   MPLACE(outsk+(256/44100), insk, dur, pre_amp, dist2sound, angle_sound, dist_mikes, inchan)
   MPLACE(outsk+0.5, insk, dur, pre_amp,dist2sound*1.3, angle_sound+270, dist_mikes, inchan)
   
   RVB(0, 0, dur+rvbtime+0.5, 1)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MROOM](MROOM.html),
[PLACE](PLACE.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
