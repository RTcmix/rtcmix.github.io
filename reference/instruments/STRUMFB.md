---
title: STRUMFB()
layout: ref
---

## STRUMFB

Extended karplus-strong ("plucked string") physical model with distortion and feedback.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**STRUMFB**(outsk, dur, AMP, PITCH, FEEDBACK\_PITCH, squish,
FUND\_DECAY, NYQ\_DECAY, DISTORTION\_GAIN, FEEDBACK\_GAIN, CLEANLEVEL,
DISTLEVEL\[, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0  = output start time (seconds)
   p1  = duration (seconds)
   p2  = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3  = pitch (Hz or oct.pc *) (see note below)
   p4  = feedback pitch (Hz or oct.pc *) (see note below)
   p5  = squish (0-10)
   p6  = fundamental decay time (seconds)
   p7  = Nyquist decay time (seconds)
   p8  = distortion gain (0-100 (or more!))
   p9  = feedback gain (0-10 -- values > 1.0 are very 'fed-back')
   p10 = clean signal level (0.0-1.0)
   p11 = distortion signal level (0.0-1.0)
   p12 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p2 (amp), p3 (pitch), p4 (feedback pitch), p6 (fundamental decay),
   p7 (Nyquist decay), p8 (distortion gain), p9 (feedback gain), p10 (clean signal level,
   p11 (distortion signal level) and p12 (pan) can receive updates from a table or
   real-time control source.

   * If the value of p3 or p4 field is < 15.0, it assumes oct.pc.  Use the pchcps
   scorefile convertor for direct frequency specification below 15.0 Hz
```

  

-----

  
**STRUMFB** is an updated version (updated from the older
[STRUM](STRUM.html) family of instruments) of the Karplus-Strong
"plucked string" algorithm, with extensions allowing distortion and
feedback based upon work by Charles Sullivan. The Karplus-Strong
plucked-string algorithm is a subtractive synthesis system featuring a
burst of white noise, a recirculating delay line, a lowpass filter, an
allpass filter, and a snazzy recursion (see Roads, 1997).

The basic idea is that a burst of noise is pushed through a delay line,
which splits its output, sending one half as output and the rest of it
back into itself after going through a lowpass and allpass filter setup.
The result is a burst of rich sound that gradually loses its higher
harmonics as it decays (as does, interestingly enough, a plucked
string).

Charlie added an additional delay with waveshaping distortion to produce
a simulation of a distorted electric guitar with feedback.

### Usage Notes

As noted above, the "PITCH" parameter (p3) can be in Hz or oct.pc form.
The decision is based upon the value being \< 15.0 (\< 15.0 will be
interpreted as oct.pc).

The "FEEDBACK\_PITCH" parameter (p4) sets up a delay corresponding to
the desired pitch for the feedback sound. This does not mean that you
will get this pitch as a result. Instead, the feedback will generally
align with some harmonic of this pitch. However, not even that is
guaranteed. This is a very non-linear parameter. Altering this pitch as
a note evolves via the pfield control system can produce interesting
changes in the output, kind of like Jimi Hendrix leaning into and away
from his amplifier (essentially he was altering the length (pitch) of
the feedback "delay line" between the amp and his guitar). This
parameter can also be in Hz or oct.pc form, like p3 above.

The "squish" parameter (p5) tells how "squishy" is the item being used
to pluck the string. Values are integers ranging from 0 to 10 The lower
the value, the harder the plucking object (0 is like plucking with a
steel pick). The higher, the more "fleshy" (fat fingers\!)

The "FUND\_DECAY" parameter (p6) sets the time for the decay of the
fundamental frequency in the synthesis algorithm. Usually this is the
same as the duration of the note (p1), but shorter times can give a
'damped' effect, where longer times can yield a more sustained string
sound. If p6 is \> p1, it's generally a good idea to apply an amplitude
envelope of some kind to prevent clicks at the end of the note.

The "NYQ\_DECAY" parameter (p7) attempts to control the decay rate of
the highest, fastest-decaying partials of the sound (i.e. the decay time
for frequencies at the top of the frequency range, the Nyquist
frequency). This can work up to a point, but realistically our
implementation of the algorithm doesn't allow for much independence
between the fundamental decay and Nyquist decay. Theoretically the ratio
of the "funddecay" and "nyqdecay" parameters effect the brightness of
the plucked note, but this effect isn't too noticeable. The "squish"
parameter does a better job of altering the plucked timbre.

"FEEDBACK\_GAIN" (p9) is a very sensitive parameter. You can set this to
virtually any value you want, but high values don't necessarily produce
much change. Often very low values (\< 0.01) are necessary for subtle
feedback sounds.

Simularly, "DISTORTION\_GAIN" (p8) can be set arbitrarily (we can go
waaaaay 'beyond 11'), but at some point it won't make much difference.

The "CLEANLEVEL" (p10) and "DISTLEVEL" (p11) can be set to mix between
the 'straight' Karplus-Strong sound and the distorted sound, if desired.

**STRUMFB** can produce either mono or stereo output.

### Sample Scores

basic use:

``` 
   rtsetparams(44100, 2)
   load("STRUMFB")
   
   dur = 7
   freq = 6.08
   decaytime = 1
   nyqdecaytime = 1
   distgain = 10
   fbgain = 0.05
   cleanlevel = 0
   distlevel = 1
   amp = 20000
   squish = 2
   
   start = 0
   fbfreq = 7.00
   STRUMFB(start, dur, amp, freq, fbfreq, squish, decaytime, nyqdecaytime,
    distgain, fbgain, cleanlevel, distlevel)
   
   start = 8
   fbfreq = 7.01
   STRUMFB(start, dur, amp, freq, fbfreq, squish, decaytime, nyqdecaytime,
    distgain, fbgain, cleanlevel, distlevel)
   
   start = 16
   fbfreq = 6.01
   STRUMFB(start, dur, amp, freq, fbfreq, squish, decaytime, nyqdecaytime,
    distgain, fbgain, cleanlevel, distlevel)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("STRUMFB")
   
   dur = 7.5
   amp = 10000
   
   pitch = cpspch(6.08)
   pitch2 = cpspch(7.00)
   freq = maketable("line", "nonorm", 1000, 0,pitch, 4,pitch, 6,pitch2, 8,pitch)
   reset(200)
   
   decaytime = 1
   nyqdecaytime = 1
   distgain = 10
   fbgain = 0.05
   fbpitch = 7.00
   squish = 2
   cleanlevel = 0
   distlevel = 1
   
   STRUMFB(0, dur, amp, freq, fbpitch, squish, decaytime, nyqdecaytime,
    distgain, fbgain, cleanlevel, distlevel)
```

  
  
fun stuff\!

``` 
   rtsetparams(44100, 2, 256)
   load("STRUMFB")
   
   dur = 7
   pitch = 7.07
   decaytime = 1
   nyqdecaytime = 1
   distgain = 10
   fbgain = 0.05
   fbpitch = 7.00
   cleanlevel = 0
   distlevel = 1
   amp = 10000
   squish = 2
   
   viblow = 4
   vibhigh = 7
   vibdepth = 5 // Hz
   vibseed = 0
   lfreq = makerandom("low", frq=10, viblow, vibhigh, vibseed)
   vibenv = maketable("line", 1000, 0,0, 1,1, 2,0)
   vib = makeLFO("sine", lfreq, vibdepth) * vibenv
   freq = cpspch(pitch) + vib
   
   env = maketable("line", 1000, 0,1, 19,1, 20,0)
   
   reset(500)
   
   STRUMFB(0, dur, amp * env, freq, fbpitch, squish, decaytime, nyqdecaytime,
    distgain, fbgain, cleanlevel, distlevel)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makerandom](../scorefile/makerandom.html),
[makeLFO](../scorefile/makeLFO.html), [MSITAR](MSITAR.html),
[STRUM](STRUM.html), [STRUM2](STRUM2.html)
