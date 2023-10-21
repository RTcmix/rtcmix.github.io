---
title: MIX()
layout: ref
---

## MIX

Mix inputs to outputs.

*in RTcmix/insts/base*  
  

-----

##### quick syntax:

**MIX**(outsk, insk, dur, AMP, p4-n: output channel assigns)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4-n = channel mix maxtrix

   p3 (amplitude) can receive dynamic updates from a table or real-time
   control source.

   rev for v4, JGG, 7/9/04
```

  

-----

  
**MIX** is the oldest and simplest of the cmix family of instruments. It
takes an input file and, well, mixes it to an output file. Since you can
change the input file as the score is parsed, you can use it to mix a
number of files together. Because of its lack of panning, **MIX** is
basically outdated for general 'stereo' purposes, and normally you will
want to use the [STEREO](STEREO.html) instrument. We include it here
mainly for historical reasons, plus it is still valuable when dealing
with more than two input or output channels or a range of [aux buses
(bus\_config)](../scorefile/bus_config.html).

### Usage Notes

The trickiest part of **MIX** is understanding how the p4-pN channel mix
matrix works. Thus:  

The number of p4-pN p-fields available is then set by the number of
input channels, and each one can be assigned to the range of output
channels available. Note that channels are counted beginning with "0".
Also Note that channels can't be "panned", only assigned to an available
output channel.

Using a "-1" in the output channel assignment (as stated above)
efectively mutes that input channel. This can be useful for extracting
single channels from a multi-channel input file.

Also note that the sample scores have no "load()" directive. **MIX** is
the main RTcmix object ("mix" in the old, disk-based cmix filled the
parallel role), and thus does not need to be dynamically-loaded. It is
'always there'. Always.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)

   rtinput("mystereosound.aiff")
   ampenv = maketable("line", 1000, 0,0, 1,1)

   MIX(0, 0, 7.0, 1*ampenv, 0, 0)

   ampenv = maketable("line", 1000, 0,0, 1,1, 2,0)
   MIX(0.1, 0, 7.0, 1*ampenv, 1, 1)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 1) // note rtsetparams() sets *output* channels

   rtinput("myquadsound.aiff")

   MIX(0, 0, 6, 1, -1, 0, -1, -1) // extracts channel 1 from the quad file
```

  
  
fun stuff\!

```cpp
   // granular-like!
   rtsetparams(44100, 2)
   
   rtinput("mysoundfile.wav")
   filedur = DUR()
   
   amp = 1.0
   env = maketable("line", 1000, 0,0, .2,1, 2,0)
   
   // to make sure these very short notes are enveloped precisely
   control_rate(10000)
   
   dur = 1
   for (outsk = 0; outsk < 14.0; outsk += dur) {
    insk = random() * filedur
    dur = random() * 0.2
   
    if (random() > 0.5)
        ch1 = 0
    else
        ch1 = 1
    if (random() > 0.5)
        ch2 = 0
    else
        ch2 = 1
   
    MIX(outsk, insk, dur, amp * env, ch1, ch2)
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [NPAN](NPAN.html),
[PAN](PAN.html), [QPAN](QPAN.html), [REVMIX](REVMIX.html),
[SPLITTER](SPLITTER.html), [STEREO](STEREO.html)
