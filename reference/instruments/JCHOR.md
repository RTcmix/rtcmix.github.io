---
title: JCHOR()
layout: ref
---

## JCHOR

Granulated chorus instrument.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**JCHOR**(outsk, insk, dur, indur, inmaintain, pitch, nvoices, MINAMP,
MAXAMP, MINWAIT, MAXWAIT, seed, inputchan, AMPENV, GRAINENV)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0  = output start time (seconds)
   p1  = input start time (seconds)
   p2  = output duration (seconds)
   p3  = input duration (seconds, not input end time)
   p4  = maintain input duration, regardless of transposition (1: yes, 0: no)
   p5  = transposition (oct.pc)
   p6  = number of voices (minimum of 1)
   p7  = minimum grain amplitude (relative multiplier of p13)
   p8  = maximum grain amplitude (relative multiplier of p13)
   p9  = minimum grain wait (seconds)
   p10 = maximum grain wait (seconds)
   p11 = seed (0-1)
   p12 = input channel
   p13 = overall amplitude (relative multiplier of input sound)
   p14 = grain envelope table (pfield-handle)

   p7 (min amp), p8 (max amp), p9 (min wait), p10 (max wait) and p13 (amp mult)
   can receive dynamic updates from a table or real-time control source.

   p14 (grain envelope) should be a reference to a pfield table-handle.

   Author:  John Gibson, 9/20/98, RT'd 6/24/99; rev for v4, 7/24/04
       based on Paul Lansky's CMIX chor instrument  and Doug Scott's TRANS code
```

  

-----

  
**JCHOR** is a random-wait chorus instrument based on Paul Lansky's old
CMIX chor instrument and Doug Scott's [TRANS](TRANS.html). Essentially
this is a granular synthesis instrument optimized to produce
'choir-like' sounds. Paul Lansky used this approach in man of his *Idle
Chatter* pieces (the chords in the background).

### Usage Notes

Because the transposition method doesn't try to maintain duration -- it
works like the speed control on a tape deck -- you have an option about
the way to handle the duration of the input read:

  - If p4 ("inmaintain") is 1, the grain length after transposition will
    be the same as that given by p3 ("indur". This means that the amount
    actually read from the input file will be shorter or longer than p3,
    depending on the transposition.
  - If p4 is 0, the segment of sound specified by p3 will be read, and
    the grain length will be shorter or longer than p3, depending on the
    transposition.
  - p7 ("MINDUR") and p8 ("MAXDUR") set the bounds for a
    randomly-determined length of each grain.
  - p9 ("MINAMP") and p10 ("MAXAMP") set the bounds for a
    randomly-determined amplitude multiplier of each grain. The overall
    synthesis amplitude is set by p2.

**JCHOR** places no limit on input duration or number of voices. It does
transpose the input signal according to the oct.pc value given in p5
("pitch").

p14 ("GRAINENV") is a pfield-handle reference to a table with the
envelope to be applied for each grain. NOTE: You will probably need to
set the reset control-rate value (see the
[reset](../scorefile/reset.html) scorefile command) fairly high for
small grains.

Output can be either mono or stereo. If it's stereo, the program
randomly distributes the voices across the stereo field.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("JCHOR")
   
   rtinput("mysound.aif")
   inchan = 0
   inskip = 0.20
   
   outdur = 10
   
   indur = 0.60
   maintain_dur = 1
   transposition = 0.07
   nvoices = 50
   minamp = 0.01
   maxamp = 1.0
   minwait = 0.00
   maxwait = 0.30
   seed = 0.9371
   
   amp = 0.5
   env = maketable("line", 1000, 0,0, 1,1, outdur-1,1, outdur,0)
   
   grainenv = maketable("window", 1000, "hanning")
   
   JCHOR(0, inskip, outdur, indur, maintain_dur, transposition, nvoices,
      minamp, maxamp, minwait, maxwait, seed, inchan, amp * env, grainenv)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("JCHOR")
   
   rtinput("mysound.wav")
   inchan = 0
   
   outdur = 16
   inskip = 0.01
   indur = 0.20
   maintain_dur = 1
   transposition = 0.02
   nvoices = 60
   minamp = 0.1
   maxamp = 1.0
   minwait = 0.00
   maxwait = 0.60
   seed = 0.9371
   
   amp = 6.0
   env = maketable("line", 1000, 0,0, 1,1, outdur-3,1, outdur,0)
   
   grainenv = maketable("window", 1000, "hanning")
   
   reset(2000)
   
   outskip = 0
   JCHOR(outskip, inskip, outdur, indur, maintain_dur, transposition, nvoices,
      minamp, maxamp, minwait, maxwait, seed, inchan, amp * env, grainenv)
   
   outskip = 2
   outdur -= outskip
   amp = 0.6
   indur = 0.90
   transposition = -0.10
   maxamp = 0.5
   maxwait = 1.0
   seed = 0.2353
   JCHOR(outskip, inskip, outdur, indur, maintain_dur, transposition, nvoices,
      minamp, maxamp, minwait, maxwait, seed, inchan, amp * env, grainenv)
```

  

-----

### See Also

[GRANSYNTH](GRANSYNTH.html), [GRANULATE](GRANULATE.html),
[JGRAN](JGRAN.html), [SGRANR](SGRANR.html), [STGRANR](STGRANR.html)
