---
title: MMESH2D()
layout: ref
---

## MMESH2D

2-dimensional mesh physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MMESH2D**(outsk, dur, AMP, nxpoints, nypoints, xpos, ypos, decay,
strike\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amplitude | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | # of X points | 2-12 | no | no | 
p4 | # of Y points | 2-12 | no | no | 
p5 | xpos | 0.0-1.0 | no | no | 
p6 | ypos | 0.0-1.0 | no | no | 
p7 | decay value | 0.0-1.0 | no | no | 
p8 | strike energy | 0.0-1.0 | no | no | 
p9 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0.5 | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author:  Brad Garton, based on code from the Synthesis ToolKit

  

-----

  
**MMESH2D** is the "Mesh2D" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the
Synthesis ToolKit.

### Usage Notes

**MMESH2D** creates some interestingly bizarre sounds. It's sort of like
tapping on a flexible metal sheet. What fun\!

Here's what was written in the original source code:

The 'mesh' is defined by p3 ("nxpoints") and p4 ("nypoints"). The
modeled mesh is then 'struck' at the position specified by p5 ("xpos")
and p6 ("ypos").

Sometimes the amplitude is very small and has to be boosted.

**MMESH2D** can produce other mono or stereo output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("MMESH2D")

   MMESH2D(0, 4.5, 3*30000, 12, 11, 0.8, 0.9, 1.0, 1.0, 0.5)

   amp = 30000
   ampenv = maketable("line", 1000, 0,0, 4,1, 5,0)
   pan = makeLFO("tri", 0.5, 0.0, 1.0)
   MMESH2D(5, 4.5, amp*ampenv*100, 10, 11, 0.7, 0.1, 1.0, 1.0, pan)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("MMESH2D")

   srand()

   st = 0
   for (i = 0; i < 150; i = i+1)
   {
      nx = irand(2, 12)
      ny = irand(2, 12)
      MMESH2D(st, 0.5, 17000, nx, ny, random(), random(), random(), random(), random())
      st = st + 0.1
   }
```

  

-----

### See Also

[AMINST](AMINST.html), [FMINST](FMINST.html),
[MBANDEDWG](MBANDEDWG.html), [MBOWED](MBOWED.html),
[MMODALBAR](MMODALBAR.html), [MSHAKERS](MSHAKERS.html),
[STRUM](STRUM.html), [STRUM2](STRUM2.html), [STRUMFB](STRUMFB.html),
[WIGGLE](WIGGLE.html)
