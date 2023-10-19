---
title: JDELAY()
layout: ref
---

## JDELAY

Simple regenerating delay-line + filter.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**JDELAY**(outsk, insk, dur, AMP, DELAYTIME, FEEDBACK, ringdowndur,
FILTFREQ, SIGMIX\[, inputchan, PAN, prefadesend, dcblock\])

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
   p2 = input duration (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = delay time (seconds)
   p5 = delay feedback (regeneration multiplier, 0-1])
   p6 = ring-down duration (seconds)
   p7 = cutoff freq for low-pass filter (Hz, 0 to disable filter)
   p8 = wet/dry mix (0: dry, 1: wet)
   p9 = input channel number [optional; default is 0]
   p10 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]
   p11 = pre-fader send (0: no, 1: yes) [optional, default is no]
   p12 = apply DC blocking filter (0: no, 1: yes) [optional, default is yes]
  
   p3 (amplitude), p4 (delay time), p5 (feedback), p7 (cutoff), p8 (wet/dry)
   and p8 (pan) can receive dynamic updates from a table or real-time control
   source.

   Author:  John Gibson, 6/23/99; rev for v4, 7/21/04
```

  

-----

  
**JDELAY** instantiates a regenerating delay, similar to
[DELAY](DELAY.html).

### Usage Notes

The differences between **JDELAY** and [DELAY](DELAY.html) are:

  - **JDELAY** uses interpolating delay line fetch and
    longer-than-necessary delay line, both of which make it sound less
    buzzy for audio-range delays
  - provides a simple low-pass filter (p7, "FILTFREQ")
  - provides control over wet/dry mix (p8, "SIGMIX")
  - provides a DC blocking filter (p12, "dcblock"). DC bias can affect
    sounds made with high regeneration setting.
  - by default the delay is "post-fader," as before, but there's a
    pfield switch (p11, "prefadesend") to make it "pre-fader." If
    pre-fader send is set to 1, sends input signal to delay line with no
    attenuation. Then p3 ("AMP") controls the entire note, including
    delay ring-down.
  - The point of the ring-down duration parameter (p6, "ringdowndur") is
    to let you control how long the delay will sound after the input has
    stopped. Too short a time, and the sound may be cut off prematurely.

**JDELAY** can produce mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1)
   load("JDELAY")

   rtinput("mysound.aif")

    JDELAY(0, 0, 4.3, 1.0, 0.35, 0.7, 5.0, 0, 1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("JDELAY")
   
   rtinput("mysound.aif")
   
   outskip = 0
   inskip = 0
   indur = DUR()
   amp = 1.0
   deltime = 1/cpspch(7.02)
   feedback = .990
   ringdur = 1
   percent_wet = 0.5
   prefadersend = 0  // if 0, sound stops abruptly; else env spans indur + ringdur
   
   env = maketable("line", 1000, 0,0, 1,1, 6,1, 10,0)
   
   cutoff = 4000
   JDELAY(outskip, inskip, indur, amp*env, deltime, feedback, ringdur,
      cutoff, percent_wet, inchan=0, pan=0.5, prefadersend)
```

  

-----

  
**SEE ALSO**  
  
[maketable](../scorefile/maketable.html), [COMBIT](COMBIT.html),
[DEL1](DEL1.html), [DELAY](DELAY.html) [PANECHO](PANECHO.html),
