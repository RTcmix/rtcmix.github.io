---
title: DEL1()
layout: ref
---

## DEL1

Simple stero delay of input signal.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**DEL1**(outsk, insk, dur, AMP, DELAYTIME, DELAYAMP\[, inputchan,
ringdowndur\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  
  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = output duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = right channel [channel 1] delay time (seconds)
   p5 = right channel amplitude multiplier (relative to left channel)
   p6 = input channel [optional, default is 0]
   p7 = ring-down duration [optional, default is first delay time value] 

   p3 (amplitude), p4 (delay time) and p5 (delay amplitude) can receive
   dynamic updates from a table or real-time control source.
```

  

-----

  
**DEL1** is a simple, non-regenerating delay instrument which takes an
input soundfile or real-time source and mixes it with a delayed copy of
itself with an optional difference in amplitude. The resulting sound is
placed left (channel 0, non-delayed) and right (channel 1, delayed) in a
stereo field.

### Usage Notes

This is a good instrument for generating a "slap-back" delay effect, or
for generating interesting spatial-placement illusions using two
speakers (very short delay times).

The point of the ring-down duration parameter is to let you control how
long the delay will sound after the input has stopped. If the delay time
is constant, **DEL1** will figure out the correct ring-down duration for
you. If the delay time is dynamic, you must specify a ring-down duration
if you want to ensure that your sound will not be cut off prematurely.

**DEL1** writes only stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2) // output has to be stereo
   load("DEL1")

   rtinput("somefile.aif")

   // delayed sound in right channel will be 0.14 seconds after the left
   DEL1(0, 0, 7, 1.0, .14, 1.0)
```

  
  
more advanced:

``` 
   set_option("full_duplex_on")
   rtsetparams(44100, 2, 512)
   load("DEL1")

   rtinput("AUDIO")

   ampenv = maketable("line", 1000, 0,0, 1,1, 16,1, 17,0)
   DEL1(0, 0, 17, 1*ampenv, 4.3, 1)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [DELAY](DELAY.html),
[PANECHO](PANECHO.html), [COMBIT](COMBIT.html), [JDELAY](JDELAY.html)
