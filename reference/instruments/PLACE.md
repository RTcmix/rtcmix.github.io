---
title: PLACE()
layout: ref
---

## PLACE

Stationary source room-simulation.

*in RTcmix/insts/std/MOVE*  
  

-----

##### quick syntax:

**PLACE**(outskip, inskip, dur, AMP, dist-xpos, angle-ypos,
(-)dist\_between\_mikes, reverb\_amp\[, input\_channel)  
  
**space**(dist\_to\_front, dist\_to\_right, -dist\_to\_back,
-dist\_to\_left, height, abs\_fact, rvbtime)  
  
**mikes**(mike\_angle, pattern\_factor)  
  
**mikes\_off**()  
  
**matrix**(amp, matrixval1, matrixval2, ... matrixval144)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
**PLACE** employs several subcommands to set the room-simulation
characteristics.

NOTE: This is an older RTcmix instrument, the newer
[MPLACE](MPLACE.html) instrument is probably better to use.  
  
  
<span id="PLACE"></span> **PLACE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | or endtime if negative | no | no | (seconds | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | distance | feet | no | no | to sound source, or x-coordinate (feet) of sound sourc | 
p5 | angle to sound source | (degrees; 0 degrees is straight in front), | no | no | 
      or y-coordinate (feet) of sound source
p6 | distance between 'mics' | stereo receivers | no | no | in the room (feet | 
      NOTE: if p6 is negative, p4/p5 will be interpreted as x- and y- coordinates,
      otherwise p4/p5 will set polar coordinates for the sound source location
p7 | amplitude of reverberation | relative multiplier of input signal | no | no | 
p8 | input channel |  -  | no | yes | default: 0 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

  
<span id="space"></span> **space**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | distance to front wall of room | feet | no | no | 
p1 | distance to right-hand wall of room | feet | no | no | 
p2 | distance to back wall of room | feet | no | no | < 0.0 | 
p3 | distance to left-hand wall of room | feet | no | no | < 0.0 | 
p4 | distance to ceiling of room | feet | no | no | 
p5 | wall absorption factor | 0-10; 0 == more 'dead', 10 == more 'live' | no | no | 
p6 | reverberation time | seconds | no | no | 

   NOTE: this subcommand is required for PLACE to function

  
<span id="mikes"></span> **mikes**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | microphone angle | degrees, 0 degrees is straight in front | no | no | 
p1 | microphone pattern | 0-1; 0 == omnidirectional, 1 == highly directional | no | no | 

   NOTE: this subcommand is optional for PLACE to function (the default is "mikes_off")

  
<span id="mikes_off"></span> **mikes\_off**  

   No pfields, this defeats the microphone angle and pattern settings to allow binaural simulation

   NOTE: this subcommand is optional for PLACE to function

  
<span id="matrix"></span> **matrix**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | total matrix gain | relative multiplier of input signal | no | no | 
p1-p145 | 12 x 12 matrix amp/feedback coefficients |  -  | no | yes | defaults to internal matrix | 

   NOTE: this subcommand is optional for PLACE to function

  

-----

  
**PLACE** is Doug Scott's room simulation RTcmix instrument. It places a
sound in a room of your design and simulates the acoustic resonances of
the room. Early reflections are computed using a ray-tracing approach
based in the distances between the source, the listening 'microphones'
and the walls. The generalized room reverberation is simulated using a
12 x 12 matrix of delay elements with feedback and filtering designed to
imitate the global reverberation response of the specified room.

NOTE: This is an older RTcmix instrument, the newer
[MPLACE](MPLACE.html) instrument is probably better to use.
<span id="usage_notes"></span>

### Usage Notes

The room design for **PLACE** depends on parameters specified in the
**setup** subcommand. The parameters work by assuming that the listener
(YOU) is placed at point \[0,0\] in a coordinate space. The locations of
the four walls are specified in this x,y coordinate system ("front",
"right", "-back", "-left"):

```cpp
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

**PLACE** sets the location of the source sound by specifying the
distance to the sound ("dist-xpos") and the angle ("angle\_ypos") of the
sound:

```cpp

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
processing by **PLACE**. The "dist\_mikes" parameter sets how far apart
the two 'listening' points are from the \[0,0\] center of the coordinate
room (you can choose to think of this is how big the listener's head
is...).

When specifying the parameters for **PLACE**, be aware that the
interpretation of the p4 and p5 pfields ("dist-xpos", "angle-ypos")
depends on whether or not p6 ("dist\_mikes") is positive or negative.
The distance to the mikes (p6) will be interpreted as a positive number
in any case, but if it is entered as negative it will cause p4 and p5 to
be interpreted as cartesian coordinates on a coordinate plane where the
center of the listener is located at \[0, 0\]. The default
interpretation is to use polar coordinates for the sound source
information (distance to sound, angle to sound).

p7 ("reverb\_amp") can be used to adjust the amount of reverberant sound
coming from the matrix reverberator relative to the direct sound and the
early wall reflections.

The optional subcommands that may be used to set the parameters for
**PLACE** include **mikes** and **mikes\_off**. **mikes** is a setup
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

**matrix** is used to replace the default set of amplitude/feedback
coefficients for the **RVB** matrix -- all 144 of them. This is tricky
to use.

**PLACE** requires stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100,  2)
   load("PLACE")
   
   rtinput("mysound.aif")
   
   space(400, 400, -400, -400, 400, 8., 10.)
   
   PLACE(0, 0, 14.3, 20, 10, 20, -1, 1.)
```

  

-----

### See Also

[DMOVE](DMOVE.html), [FREEVERB](FREEVERB.html), [GVERB](GVERB.html),
[MMOVE](MMOVE.html), [MOVE](MOVE.html), [MPLACE](MPLACE.html),
[MROOM](MROOM.html), [REV](REV.html), [REVERBIT](REVERBIT.html),
[ROOM](ROOM.html), [SROOM](SROOM.html)
