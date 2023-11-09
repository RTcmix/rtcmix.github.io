---
title: FLANGE()
layout: ref
---

## FLANGE

Moving comb or notch filter.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**FLANGE**(outsk, insk, dur, AMP, RESONANCE, maxdelay, MODDEPTH,
MODRATE, SIGNALMIX\[, FLANGETYPE, inputchan, PAN, ringdowndur,
MODWAVETABLE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | (seconds) | no | no | 
p1 | input start time | (seconds) | no | no | 
p2 | duration | (seconds) | no | no | 
p3 | amplitude multiplier | (relative multiplier of input signal) | yes | no | 
p4 | resonance | (can be negative) | yes | no | 
p5 | maximum delay time | (seconds) | no | no | determines lowest pitch; try: 1.0 / cpspch(8.00) | 
p6 | modulation depth | (0 - 100%) | yes | no | 
p7 | modulation rate | (Hz) | yes | no | 
p8 | wet/dry mix | (0: dry --> 1: wet) | yes | yes | default is 0.5 | 
p9 | flanger type | ("IIR" is IIR comb, "FIR" is FIR notch; or numeric codes for the flanger type (0: "IIR", 1: "FIR") | yes* | yes | default is "IIR" * only dynamic when using numeric codes| 
p10 | input channel |  -  | no | yes | default is 0 | 
p11 | pan | (0-1 stereo; 0.5 is middle) | no | yes | default is 0.5 | 
p12 | ring-down duration |  -  | no | yes | default is resonance value | 
p13 | modulator wavetable | reference to a pfield table-handle | no | yes | defaults to sine wave | 


   Author: John Gibson, 7/21/99; rev for v4, JGG, 7/24/04

  

-----

  
**FLANGE** implements a relatively standard flanging (short moving
delay), using either notch or comb filter.

### Usage Notes

**FLANGE** is a moving short delay-line, generating that ever-popular
'flanging' effect. The flanger can be set to use a notch or a comb
filter, and the modulation waveform, depth, and speed are all
user-controlled.

the "RESONANCE" parameter (p4) may be positive or negative. If negative,
the signal will be inverted each time it is sent through the
regenerative delay line. The effect is slightly different.

p7 ("MODRATE") is usually a low-frequency oscillator (\< 20 Hz), but it
doesn't have to be.

The type of flanger (p9, "FLANGETYPE") may only be changed dynamically
if it is specified using numeric codes.

The point of the ring-down duration parameter (p12) is to let you
control how long the flanger will ring after the input has stopped. If
you set p12 to zero, then **FLANGE** will try to figure out the correct
ring-down duration for you. This will almost always be fine. However, if
resonance is dynamic, there are cases where **FLANGE**'s estimate of the
ring duration will be too short, and your sound will cut off
prematurely. Use p12 to extend the duration.

The output of **FLANGE** can be either mono or stereo.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("FLANGE")
   
   rtinput("mysound.wav")

   inchan = 0
   inskip = 0
   dur = DUR()
   amp = 1.0
   
   resonance = 0.06
   lowpitch = 7.00
   moddepth = 70
   modspeed = maketable("line", "nonorm", 100, 0,4, 1,.1)
   wetdrymix = 0.5
   flangetype = "IIR"
   pan = 0.5
   
   maxdelay = 1.0 / cpspch(lowpitch)
   
   FLANGE(outskip=0, inskip, dur, amp, resonance, maxdelay, moddepth, modspeed,
      wetdrymix, flangetype)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("FLANGE")
   
   rtinput("mysound.aif")

   inchan = 0
   
   start = 0
   inskip = 0
   dur = DUR()
   amp = 0.7
   
   resonance = 0.10
   lowpitch = 8.00
   moddepth = 80
   modspeed = 0.5
   wetdrymix = 0.5
   flangetype = "IIR"
   rightchandelay = 0.08
   ringdur = 0      // let inst figure it out
   
   waveform = maketable("wave", 1000, "sine")
   
   maxdelay = 1.0 / cpspch(lowpitch)
   FLANGE(start, inskip, dur, amp, resonance, maxdelay, moddepth, modspeed,
      wetdrymix, flangetype, inchan, pan=1, ringdur, waveform)
   
   // 45 deg out of phase with left chan sine
   waveform = maketable("wave3", 1000, 1, 1, 45)
   
   start += rightchandelay
   maxdelay *= 1.0001
   FLANGE(start, inskip, dur, amp, resonance, maxdelay, moddepth, modspeed,
      wetdrymix, flangetype, inchan, pan=0, ringdur, waveform)
```

  
  
fun stuff\!

```cpp
   /* This script imposes a trill of a major 2nd onto the input file. */
   rtsetparams(44100, 2)
   load("FLANGE")
   
   rtinput("mysound.aif")

   inchan = 0
   inskip = 0
   dur = DUR()
   amp = 0.4
   
   resonance = 1.0 /* how "ringy" are trill pitches? */
   lowpitch = 8.00 /* lower pitch of major 2nd */
   moddepth = 11.5 /* somehow makes a major 2nd above low pitch  ;-) */
   modspeed = 6.0  /* speed of trill */
   wetdrymix = 0.4 /* how prominent is trill? */
   
   // make an "ideal" square wave
   waveform = maketable("wave", 1000, "square")
   
   maxdelay = 1.0 / cpspch(lowpitch)
   
   FLANGE(0, inskip, dur, amp, resonance, maxdelay, moddepth, modspeed,
      wetdrymix, "IIR", inchan, pan=1, ringdur=0, waveform)
   
   FLANGE(.1, inskip, dur, amp, resonance, maxdelay, moddepth, modspeed,
      wetdrymix, "IIR", inchan, pan=0, ringdur=0, waveform)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [AM](AM.html),
[COMBIT](COMBIT.html), [DELAY](DELAY.html), [JDELAY](JDELAY.html),
[MOCKBEND](MOCKBEND.html), [MULTICOMB](MULTICOMB.html),
[PANECHO](PANECHO.html), [SHAPE/a\>,](SHAPE.html) TRANS/a\>
