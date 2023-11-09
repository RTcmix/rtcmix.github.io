---
title: STRUM2()
layout: ref
---

## STRUM2

Tuned Karplus-Strong ("plucked string") physical model.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**STRUM2**(outsk, dur, AMP, PITCH, squish, decay\_time\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | (seconds) | no | no | 
p1 | duration | (seconds) | no | no | 
p2 | amplitude | (absolute, for 16-bit soundfiles: 0-32768) | yes | no | 
p3 | pitch | (Hz or oct.pc)* | yes | no | *(see note below | 
p4 | squish | (0-10) | no | no | 
p5 | decay time | (seconds) | no | no | 
p6 | pan | (0-1 stereo; 0.5 is middle) | yes | yes | default is 0 | 

   * If the value of p3 field is < 15.0, it assumes oct.pc.  Use the pchcps
   scorefile convertor for direct frequency specification below 15.0 Hz.

  

-----

  
**STRUM2** is an updated version (updated from the older
[STRUM](STRUM.html) family of instruments) of the Karplus-Strong
"plucked string" algorithm. The Karplus-Strong plucked string algorithm
is a subtractive synthesis system featuring a burst of white noise, a
recirculating delay line, a lowpass filter, an allpass filter, and a
snazzy recursion (see Roads, 1997).

The basic idea is that a burst of noise is pushed through a delay line,
which splits its output, sending one half as output and the rest of it
back into itself after going through a lowpass and allpass filter setup.
The result is a burst of rich sound that gradually loses its higher
harmonics as it decays (as does, interestingly enough, a plucked
string).

### Usage Notes

As noted above, the "pitch" parameter (p3) can be in Hz or oct.pc form.
The decision is based upon the value being \< 15.0 (\< 15.0 will be
interpreted as oct.pc).

The "squish" parameter (p4) tells how "squishy" is the item being used
to pluck the string. Values are integers ranging from 0 to 10 The lower
the value, the harder the plucking object (0 is like plucking with a
steel pick). The higher, the more "fleshy" (fat fingers\!)

The "decay\_time" parameter (p5) sets the time for the decay of the
fundamental frequency in the synthesis algorithm. Usually this is the
same as the duration of the note (p1), but shorter times can give a
'damped' effect, where longer times can yield a more sustained string
sound. If p5 is \> p1, it's generally a good idea to apply an amplitude
envelope of some kind to prevent clicks at the end of the note.

You may note that the older [STRUM](STRUM.html) instruments also had a
"nyquist decay" parameter. It didn't work well, and timbral variation is
much more effective using the "squish" parameter.

**STRUM2** can produce either mono or stereo output.

### Sample Scores

basic use:

```cpp
   rtsetparams(44100, 2)
   load("STRUM2")

   STRUM2(0, 3.4, 20000, 8.00, 1, 1.0, 0.5)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("STRUM2")

   srand(4)

   makegen(1, 24, 1000, 0,1, 1,1) // amplitude envelope
   pitches = { 7.00, 7.02, 7.05, 7.07, 7.10, 8.00, 8.07 }
   lpitches = len(pitches)

   for (st = 0; st < 15; st = st + 0.1) {
      pindex = trand(0, lpitches)
      pitch = pitches[pindex]
      STRUM2(st, 1.0, 10000, pitch, 1, 1.0, random())
   }
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("STRUM2")

   pitchtab = { 7.00, 7.02, 7.05, 7.07, 7.10, 8.00, 8.07 }
   pitchtablen = len(pitchtab)

   maxpitch = 0.02
   gliss = maketable("line", "nonorm", 1000, 0,0, 0.2,octpch(maxpitch), 2,0)

   srand(7)
   
   for (st = 0; st < 15; st = st + 0.2) {
      pindex = trand(0, pitchtablen)
      pitch = pitchtab[pindex]
      pitch = octpch(pitch)
      freq = makeconverter(pitch + gliss, "cpsoct")
      stereo = random()
      STRUM2(st, 1.0, 10000, freq, 1, 1, stereo)
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makeconverter](../scorefile/makeconverter.html), [MSITAR](MSITAR.html),
[STRUM](STRUM.html), [STRUMFB](STRUMFB.html)
