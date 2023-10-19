---
title: GRANSYNTH()
layout: ref
---

## GRANSYNTH

Simple granular synthesis.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**GRANSYNTH**(outsk, dur, AMP, WAVETABLE, GRAINENV, GRAINHOP,
OUTTIMEJITTER, MINDUR, MAXDUR, MINAMP, MAXAMP, PITCH\[, TRANSPTABLE,
PITCHJITTER, seed, MINPAN, MAXPAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0  = output start time (seconds)
   p1  = total duration (seconds)
   p2  = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3  = oscillator waveform table
   p4  = grain envelope table
   p5  = grain hop time (seconds, time between successive grains)
   p6  = grain output time jitter (seconds)
   p7  = grain duration minimum (seconds)
   p8  = grain duration maximum (seconds)
   p9  = grain amplitude multiplier minimum (relative multiplier of p2)
   p10 = grain amplitude multiplier maximum (relative multiplier of p2)
   p11 = grain pitch (linear octaves)
   p12 = grain transposition collection (oct.pc) [optional; default no transpositions applied]
   p13 = grain pitch jitter (linear octaves or oct.pc (if p12 used)) [optional; default no pitch jitter applied]
   p14 = random seed (integer) [optional; if missing, uses system clock]
   p15 = grain pan minimum (0-1 stereo; 0.5 is middle) [optional; default 0.0]
   p16 = grain pan maximum (0-1 stereo; 0.5 is middle) [optional; default 1.0]


   p2 (amplitude), p5 (grain hop), p6 (output time jitter), p7 (minium grain duration),
   p8 (maximum grain duration), p9 (minimum grain amplitude), p10 (maximum grain amplitude),
   p11 (pitch), p13 (pitch jitter), p15 (grain pan minimum) and p16 (grain pan maximum)
   can receive dynamic updates from a table or real-time control source.

   p3 (wavetable), p4 (grain envelope) and p12 (if used), should be references to pfield table-handles.

   Author:  John Gibson, 2/8/05
```

  

-----

  
**GRANSYNTH** is a time-varying granular synthesis instrument, similar
to some of Mara Helmuth's instruments (see [SGRANR](SGRANR.html) and
[STGRANR](STGRANR.html)). This instrument is capable of many interesting
and subtle effects.

### Usage Notes

Parameters that determine the character of individual grains:

  - p3 ("WAVETABLE") is the oscillator waveform to use in each grain.
  - p4 ("GRAINENV") is a pfield-handle reference to a table with the
    envelope to be applied for each grain. NOTE: You will probably need
    to set the reset control-rate value (see the
    [reset](../scorefile/reset.html) scorefile command) fairly high for
    small grains.
  - p5 ("GRAINHOP") is the time between successive grains. This is the
    inverse of grain density (grains per second); you can use
    [makefilter(..., "invert")](../scorefile/makefilter.html#invert) to
    convert a table or real-time control source from density to hop
    time.
  - p6 ("OUTTIMEJITTER") sets the maximum randomly determined amount to
    add or subtract from the output start time for a grain, which is
    controlled by p5.
  - p7 ("MINDUR") and p8 ("MAXDUR") set the bounds for a
    randomly-determined length of each grain.
  - p9 ("MINAMP") and p10 ("MAXAMP") set the bounds for a
    randomly-determined amplitude multiplier of each grain. The overall
    synthesis amplitude is set by p2.
  - p11 ("PITCH") sets the base pitch of each grain, expressed in linear
    octaves (see the [pitch converter](../scorefile/octcps.html) page
    for more information).
  - p12 ("TRANSPTABLE"), if specified, contains a list of transpositions
    (in oct.pc) from which to select randomly. The values from the table
    are used to transpose p11. The default is 0, meaning all grains will
    have a base pitch of p11, subject to the PITCHJITTER set by p12.
    This table cannot be updated dynamically.
  - p13 ("PITCHJITTER") sets the maximum randomly determined amount to
    add or subtract from the current pitch (in linear octaves). If the
    p12 ("TRANSPTABLE" collection) is active, then PITCHJITTER controls
    how much of the collection to choose from. In this case, PITCHJITTER
    is an oct.pc value. For example, if the collection is \[0.00, 0.02,
    0.05, 0.07\], then a PITCHJITTER value of 0.05 will cause only the
    first 3 transpositions to be chosen, whereas a PITCHJITTER value of
    0.07 would cause all 4 to be chosen.
  - p15 ("MINPAN") and p16 ("MAXPAN") set the bounds for a
    randomly-determined pan amount (0-1 stereo; 0.5 is middle) for each
    synthesized grain.

**GRANSYNTH** can produce either stereo or mono output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2, 128)
   load("GRANSYNTH")
   
   dur = 30
   
   amp = maketable("line", 1000, 0,0, 1,1, 2,0.5, 3,1, 4,0)
   wave = maketable("wave", 2000, 1, .5, .3, .2, .1)
   granenv = maketable("window", 2000, "hanning")
   hoptime = maketable("line", "nonorm", 1000, 0,0.01, 1,0.002, 2,0.05)
   hopjitter = 0.0001
   mindur = .04
   maxdur = .06
   minamp = maxamp = 1
   pitch = maketable("line", "nonorm", 1000, 0,6, 1,9)
   transpcoll = maketable("literal", "nonorm", 0, 0, .02, .03, .05, .07, .10)
   pitchjitter = 1
   
   st = 0
   GRANSYNTH(st, dur, amp*7000, wave, granenv, hoptime, hopjitter,
      mindur, maxdur, minamp, maxamp, pitch, transpcoll, pitchjitter, 14, 0, 0)
   
   st = st+0.14
   pitch = pitch+0.002
   GRANSYNTH(st, dur, amp*7000, wave, granenv, hoptime, hopjitter,
      mindur, maxdur, minamp, maxamp, pitch, transpcoll, pitchjitter, 21, 1, 1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2, 128)
   load("GRANSYNTH")
   
   dur = 60
   
   amp = makeconnection("mouse", "y", 50, 70, 10, 60, "amp", "dB")
   amp = makeconverter(amp, "ampdb")
   
   wavetab = maketable("wave", 2000, 1, .5, .3, .2, .1)
   
   envtab = maketable("window", 2000, "hanning")
   
   outjitter = 0.0001
   
   density = makeconnection("mouse", "x", 1, 500, 1, 10, "density")
   hoptime = 1.0 / density
   
   mindur = .05
   maxdur = mindur
   minamp = maxamp = 1
   
   pitch = makeconnection("mouse", "y", 6, 8, 6, 10, "pitch", "linoct")
   
   transpcoll = maketable("literal", "nonorm", 0, 0, .02, .03, .05, .07, .10)
   pitchjitter = 1
   
   seed = 1
   
   st = 0
   GRANSYNTH(st, dur, amp, wavetab, envtab, hoptime, outjitter,
      mindur, maxdur, minamp, maxamp, pitch, transpcoll, pitchjitter, seed, 0, 0)
   
   st += 0.01
   seed += 1
   pitch = pitch + 0.002
   GRANSYNTH(st, dur, amp, wavetab, envtab, hoptime, outjitter,
      mindur, maxdur, minamp, maxamp, pitch, transpcoll, pitchjitter, seed, 1, 1)
```

  

-----

### See Also

[GRANULATE](GRANULATE.html), [JCHOR](JCHOR.html), [JGRAN](JGRAN.html),
[SGRANR](SGRANR.html), [STGRANR](STGRANR.html)
