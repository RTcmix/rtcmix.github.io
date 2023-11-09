---
title: CLAR()
layout: ref
---

## CLAR

Physical model clarinet.

*in RTcmix/insts/std*  
*NOTE: This instrument has largely been superceded by the
[MCLAR](MCLAR.html)* instrument.  
  

-----

##### quick syntax:

**CLAR**(outsk, dur, noiseamp, length1 (samples), length2 (samples),
outputamp, d2gain\[, pan\])


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | noise amplitude | 0-1 | no | no | 
p3 | length 1 | 1-500, length in samples | no | no | 
p4 | length 2 | 1-500, length in samples | no | no | 
p5 | output amplitude | absolute, for 16-bit soundfiles: 0-32768 | no | no | 
p6 | d2 gain | 0-1 | no | no | 
p7 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 

   No pfield-enabled control of parameters is implemented for this instrument.  Older makegen()-style control
   may be used if desired.

  

-----

  
**CLAR** is a physical modeling algorithm for a clarinet. *Physical
modeling* is a synthesis paradigm whereby the computer synthesizes sound
not according to the spectrum of the desired output, but rather based on
a description of the *physical process* which makes the sound (to whit:
a violin's physical model would include a system describing the physics
of a vibrating string, coupled with a quantified description of the
effects of bow pressure, string vibrato, etc.). physical modeling
instruments, therefore, are simulated and controlled by expert systems
describing cause-and-effect relationships of specific performance
actions on corresponding acoustic results. (adapted from Cook, 1992,
1995).

### Usage Notes

This is one of the first physical-model instruments implemented. It is
here primarily for 'historical' reasons, the [MCLAR](MCLAR.html)
instrument is a better model to use. For example, to specfy the pitch in
**CLAR**, it is necessary to find the right combination of delay lengths
and the d2gain parameter. This isn't really intuitive... Also, p-field
control has not been implemented for this instrument.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("CLAR")

   CLAR(0, 1, 0.02, 78, 31, 7000, 0)
   CLAR(1, 1, 0.02, 35, 4, 7000, 0)
   CLAR(2, 1, 0.02, 35, 9, 7000, 0)
   CLAR(3, 1, 0.02, 51, 20, 7000, 0.5)
```

  
  
more advanced:

```cpp
   rtsetparams(44100, 2)
   load("CLAR")

   // this is the old makegen() system.  Generally use the
   // maketable() system for dynamic control.  CLAR doesn't use it
   makegen(1, 24, 1000, 0, 1, 1, 1)
   makegen(2, 24, 1000, 0, 1, 1, 1)

   d2 = 0
   for (start = 0; start < 10; start = start + 0.5) {
      CLAR(start, 0.5, 0.02, 69, 34, 7000, d2)
      d2 = d2 + 0.05
   }
```

  

-----

### See Also

[MCLAR](MCLAR.html), [MBLOWHOLE](MBLOWHOLE.html), [MBRASS](MBRASS.html),
[METAFLUTE](METAFLUTE.html), [MSAXOFONY](MSAXOFONY.html)
