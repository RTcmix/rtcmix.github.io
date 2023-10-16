---
title: GRANULATE()
layout: ref
---

## GRANULATE

Granulation of sound stored in a table.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**GRANULATE**(outsk, INSK, dur, AMP, INPUTTABLE, inchans, INTABLECHAN,
INSTART, INEND, WRAPAROUND, TRAVERSERATE, GRAINENV, GRAINHOP,
INTIMEJITTER, OUTTIMEJITTER, MINDUR, MAXDUR, MINAMP, MAXAMP, PITCH\[,
TRANSPTABLE, PITCHJITTER, seed, MINPAN, MAXPAN, INTERPTYPE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0  = output start time (seconds)
   p1  = input start time (seconds, will be constrained to window, p5-6)
   p2  = total duration (seconds)
   p3  = amplitude multiplier (relative multiplier of input sound)
   p4  = input sound table (pfield-handle)
   p5  = number of channels in sample table
   p6  = input channel of sample table (seconds)
   p7  = input window start time (seconds)
   p8  = input window end time (seconds)
   p9  = wraparound (1: yes,  0: no (usually use 1))
   p10 = traversal rate (speed multiplier, 1.0 == normal, -1.0 == backwards)
   p11 = grain envelope table (pfield-handle)
   p12 = grain hop time (seconds, time between successive grains)
   p13 = grain input time jitter (seconds)
   p14 = grain output time jitter (seconds)
   p15 = grain duration minimum (seconds)
   p16 = grain duration maximum (seconds)
   p17 = grain amplitude multiplier minimum (relative multiplier of p3)
   p18 = grain amplitude multiplier maximum (relative multiplier of p3)
   p19 = grain transposition (in linear octaves, relative to 0)
      [optional; default: no transposition]
   p20 = grain transposition collection (oct.pc) [optional; default no transpositions applied]
   p21 = grain transposition jitter (linear octaves or oct.pc (if p20 used))
      [optional; if missing, no transposition jitter]
   p22 = random seed (integer) [optional; default: system clock]
   p23 = grain pan minimum (0-1 stereo; 0.5 is middle) [optional; default 0.0]
   p24 = grain pan maximum (0-1 stereo; 0.5 is middle) [optional; default 0.0]
   p25 = interpolation type (0: 2nd-order interpolation, 1: 3rd-order interpolation) [optional; default is 0]

   p2 (insk), p4 (amplitude), p6 (input channel #), p7 (input window start), p8 (input window end),
   p9 (wraparound flag), p10 (speed), p12 (grain hop), p13 (input time jitter),
   p14 (output time jitter), p15 (minimum grain duration), p16 (maximum grain duration),
   p17 (minimum grain amplitude), p18 (maximum grain amplitude), p19 (grain transposition),
   p21 (pitch jitter), p23 (grain pan minimum), p24 (grain pan maximum), and
   p25 (interpolation type) can receive dynamic updates from a table or real-time control source.

   p4 (input sound table), p11 (grain envelope) and p20 (if used), should be references to pfield table-handles.

   Author:  John Gibson, 1/29/05
```

  

-----

  
**GRANULATE** is differs from other signal-processing instruments in
RTcmix in that it operates upon an input table (see
[maketable("soundfile", ...)](../scorefile/maketable.html#soundfile)
scorefile command for one possible way to load this table) instead of
granulating an input stream or an existing soundfile. This gives it some
flexibility and speed that other granulators may lack, although the
sound needs to be loaded into memory (the table) first.

### Usage Notes

p1 ("INSK") will be constrained to window, the bounds set by p7 and p8.
NOTE: Unlike other instruments, this applies to a table, not to an input
sound file.

The p4 parameter ("INPUTTABLE") is a pfield-handle set to a table
containing the entirety of an input sound to be granualted. This is set
by using the [maketable("soundfile",
...)](../scorefile/maketable.html#soundfile) scorefile command.

The "INSTART" (p7) and "INEND" (p8) parameters refer to the portion of
the input sound table read by the granulator. When the granulator
reaches the end of the window, it wraps around to the beginning, unless
p9 ("WRAPAROUND") is false (0). In that case, please note that any data
coming from time-varying tables may not span the entire table;
processing will stop.

It's best to arrange for the window start to be a little after the sound
table start, and for the window end to be a little before the table end.

p10 ("TRAVERSERATE") sets the speed for reading through the input sound.
Here are some sample values:

``` 
   0     no movement
   1     move forward at normal rate
   2.5   move forward at a rate that is 2.5 times normal
   -1    move backward at normal rate
```

The following parameters determine the character of individual grains.

  - p11 ("GRAINENV") is a pfield-handle reference to a table with the
    envelope to be applied for each grain. NOTE: You will probably need
    to set the reset control-rate value (see the
    [reset](../scorefile/reset.html) scorefile command) fairly high for
    small grains.
  - p12 ("GRAINHOP") is the time between successive grains. This is the
    inverse of grain density (grains per second); you can use
    [makefilter(..., "invert")](../scorefile/makefilter.html#invert) to
    convert a table or real-time control source from density to hop
    time.
  - p13 ("INTIMEJITTER) sets the maximum randomly determined amount to
    add or subtract from the input start time for a grain, which is
    controlled by p1 and the traversal rate (speed), p10.
  - p13 ("OUTTIMEJITTER") sets the maximum randomly determined amount to
    add or subtract from the output start time for a grain, which is
    controlled by the grain hop time (p12).
  - p15 ("MINDUR") and p16 ("MAXDUR") set the bounds for a
    randomly-determined length of each grain.
  - p17 ("MINAMP") and p18 ("MAXAMP") set the bounds for a
    randomly-determined amplitude multiplier of each grain. The overall
    synthesis amplitude is set by p2.
  - p19 ("PITCH") sets the base pitch of each grain, expressed in linear
    octaves (see the [pitch converter](../scorefile/octcps.html) page
    for more information).
  - p20 ("TRANSPTABLE"), if specified, contains a list of transpositions
    (in oct.pc) from which to select randomly. The values from the table
    are used to transpose p11. The default is 0, meaning all grains will
    have a base pitch of p11, subject to the PITCHJITTER set by p12.
    This table cannot be updated dynamically.
  - p21 ("PITCHJITTER") sets the maximum randomly determined amount to
    add or subtract from the current pitch (in linear octaves). If the
    p12 ("TRANSPTABLE" collection) is active, then PITCHJITTER controls
    how much of the collection to choose from. In this case, PITCHJITTER
    is an oct.pc value. For example, if the collection is \[0.00, 0.02,
    0.05, 0.07\], then a PITCHJITTER value of 0.05 will cause only the
    first 3 transpositions to be chosen, whereas a PITCHJITTER value of
    0.07 would cause all 4 to be chosen.
  - p23 ("MINPAN") and p24 ("MAXPAN") set the bounds for a
    randomly-determined pan amount (0-1 stereo; 0.5 is middle) for each
    synthesized grain.
  - p25 ("INTERPTYPE") allows you use 3rd-order interpolation, instead
    of 2nd-order, when transposing. This is more CPU-intensive, and
    often doesn't make a noticeable difference. If you are doing some
    major pitch-shifting, it can produce a smoother sound, however.

**GRANULATE** can produce either stereo or mono output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2, 512)
   load("GRANULATE")
   
   dur = 60
   amp = ampdb(-12)
   env = maketable("curve", 1000, 0,0,1, 2,1,0, 7,1,-1, 10,0)
   
   fname = "mysound.wav"
   intab = maketable("soundfile", "nonorm", 0, fname)
   filedur = 1.578957
   numchans = 1
   inchan = 0
   
   inskip = 0.45
   winstart = 0.1
   winend = filedur - 0.1
   wrap = 1
   
   travrate = 0.01
   
   envtab = maketable("window", 1000, "hanning")
   
   injitter = 0.0
   outjitter = 0.001
   hoptime = 0.006
   mindur = hoptime * 22
   maxdur = mindur
   minamp = maxamp = 1.0
   transp = -0.07
   transpcoll = 0
   transpjitter = 0.0
   seed = 1
   minpan = 0.3
   maxpan = 0.7
   
   GRANULATE(0, inskip, dur, amp * env, intab, numchans, inchan, winstart, winend,
      wrap, travrate, envtab, hoptime, injitter, outjitter, mindur, maxdur,
      minamp, maxamp, transp, transpcoll, transpjitter, seed, 0, 0)

   seed += 1
   GRANULATE(0, inskip, dur, amp * env, intab, numchans, inchan, winstart, winend,
      wrap, travrate, envtab, hoptime, injitter, outjitter, mindur, maxdur,
      minamp, maxamp, transp, transpcoll, transpjitter, seed, 1, 1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2, 512)
   load("GRANULATE")
   
   dur = 360
   amp = ampdb(-12)
   
   fname = "mysound.wav"
   intab = maketable("soundfile", "nonorm", 0, fname)
   filedur = 1.578957
   numchans = 1
   inchan = 0
   
   inskip = 0.45
   winstart = 0.01
   winend = filedur - 0.01
   wrap = 1
   
   travrate = makeconnection("mouse", "x", min=-2, max=2, dflt=0, lag=30, "rate")
   
   envtab = maketable("window", 1000, "hanning")
   
   injitter = 0.0
   outjitter = 0.001
   hoptime = 0.006
   mindur = hoptime * 18
   maxdur = mindur
   minamp = maxamp = 1.0
   
   transp = makeconnection("mouse", "y", min=-1, max=1, dflt=0, lag=20,
                                                         "transp", "oct")
   
   transpcoll = 0
   transpjitter = 0.0
   
   seed = 1
   minpan = 0.2
   maxpan = 0.8
   
   GRANULATE(0, inskip, dur, amp, intab, numchans, inchan, winstart, winend,
      wrap, travrate, envtab, hoptime, injitter, outjitter, mindur, maxdur,
      minamp, maxamp, transp, transpcoll, transpjitter, seed, 0, 0)

   seed += 1
   GRANULATE(0, inskip, dur, amp, intab, numchans, inchan, winstart, winend,
      wrap, travrate, envtab, hoptime, injitter, outjitter, mindur, maxdur,
      minamp, maxamp, transp, transpcoll, transpjitter, seed, 1, 1)
```

  

-----

### See Also

[GRANSYNTH](GRANSYNTH.html), [JCHOR](JCHOR.html), [JGRAN](JGRAN.html),
[SGRANR](SGRANR.html), [STGRANR](STGRANR.html)
