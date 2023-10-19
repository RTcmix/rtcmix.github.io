---
title: TRANS()
layout: ref
---

## TRANS

Pitch-transposiion using cubic spline interpolation.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**TRANS**(outsk, insk, dur, AMP, TRANSP\[, inputchan, PAN\])

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
   p2 = output duration (or endtime if negative) (seconds)
   p3 = amplitude multiplier (relative multiplier of input signal)
   p4 = interval of transposition (oct.pc)
   p5 = input channel [optional; default is 0]
   p6 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (amplitude), p4 (transposition) and p6 (pan) can receive dynamic updates
   from a table or real-time control source.

   Author: Doug Scott; rev. by John Gibson, 2/29/00;  rev. for v4 by JG, 3/27/05
```

  

-----

  
**TRANS** transposes the input for the specified output duration (p2),
starting at the input start time (p1). **TRANS** uses second-order
polynomial interpolation to accomplish this.
<span id="usage_notes"></span>

### Usage Notes

**TRANS** does not maintain the input duration, so it's sort of like
changing tape speed. To transpose down, it interpolates samples between
existing ones; to transpose up, it discards some existing samples. When
transposing up, then, it must consume more than outdur seconds of
samples, and this means that it's possible to reach the end of the input
file. **TRANS** will stop processing if that occurs.

This also means that you can use this instrument only with input from a
sound file, not with a real-time input (microphone or aux bus) -- at
least not without hearing clicks. (That's because you can't read samples
that haven't happened yet.)

The transposition is given in oct.pc notation, so that a "TRANSP" (p4)
value of 0.01 will transpose the input up by one semitone, a value of
-0.07 will transpose down by a fifth, and a value of 1.0225 will shift
the signal up by an octave + a whole tone + a quarter tone.

Be careful when dynamically updating the transposition as the mod-12
arithmetic used in oct.pc notation may yield undesired results (i.e.
going linearly down from 8.00, the next step might be 7.99 which is 99
semitones above octave 7 in oct.pc notation). It is best to work with
linear octaves or direct frequency (Hz) specification for
control-envelope changes.

Because you specify the output duration for the note, you will need to
calculate how long a given input will shift depending on the
transposition if you want to process an entire input event. You can do
this using the [translen](../scorefile/translen.html) scorefile command.

**TRANS** can produce both mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 2)
   load("TRANS")

   rtinput("mysound.aif")

   trans = -0.02
   // do both channels of a stereo input file
   TRANS(outskip=1, inskip=2, dur=4, amp=1, trans, inchan=0, pan=0)
   TRANS(outskip=1, inskip=2, dur=4, amp=1, trans, inchan=1, pan=1)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("TRANS")
   
   rtinput("mysound.aif")

   start = 0
   inskip = 0
   dur = 7.8
   amp = 1.0
   ampenv = maketable("line", 1000, 0,0, 1,1, 7,1, 7.8,0)

   low = octpch(-0.05)
   high = octpch(0.03)
   transp = maketable("line", "nonorm", 1000, 0,0, 1,low, 3,high)
   transp = makeconverter(transp, "pchoct")

   /*
   This transposition starts at 0, moves down by a perfect fourth (-0.05),
   then up to a minor third (0.03) above the starting transposition.  The
   table is expressed in linear octaves, then converted to octave.pc by the
   call to makeconverter.
   */
   TRANS(start, inskip, dur, amp*ampenv, transp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconverter](../scorefile/makeconverter.html),
[MOCKBEND](MOCKBEND.html), [SCRUB](SCRUB.html), [TRANS3](TRANS3.html),
[TRANSBEND](TRANSBEND.html), [PVOC](PVOC.html)
