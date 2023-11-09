---
title: WAVETABLE()
layout: ref
---

## WAVETABLE

Wavetable oscillator synthesis instrument.

*in RTcmix/insts/base*  
  

-----

##### quick syntax:

**WAVETABLE**(outsk, dur, AMP, PITCH\[, PAN, WAVETABLE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amp | absolute, for 16-bit soundfiles: 0-32768 | yes | no | 
p3 | pitch | Hz or oct.pc * | yes | no | see note below | 
p4 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0 | 
p5 | reference to wavetable |  -  | yes | yes | defaults to sine wave | 

   p2 (amplitude), p3 (pitch) and p4 (pan) can receive dynamic updates
   from a table or real-time control source.

   p5 (wavetable), if used, should be a reference to a pfield table-handle.

   * If the value of p3 field is < 15.0, it assumes oct.pc.  Use the pchcps
   scorefile convertor for direct frequency specification below 15.0 Hz.

  

-----

  
**WAVETABLE** is one of the most basic synthesis instruments available
in RTcmix. It works by using a 'wavetable' template (usually constructed
using the [maketable("wave" ...)](../scorefile/maketable.html#wave)
scorefile command) to determine the waveform used to create the sound.
If no template is given (p5), then an internal sine-wave template is
used.

**WAVETABLE** can produce mono or stereo output

### Usage Notes

oct.pc format generally will not work as you expect for p3 (pitch) if
the pfield changes dynamically because of the 'mod 12' aspect of the
pitch-class (.pc) specification. Use direct frequency (hz) or linear
octaves instead.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("WAVETABLE")

   // this will make 278.0 hz sine wave tone for 3.4 seconds
   WAVETABLE(0, 3.4, 20000, 278.0)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("WAVETABLE")

   wave = maketable("wave", 1000, "saw")

   // this will make two notes a 5th apart (one slightly delayed)
   // using a sawtooth waveform
   WAVETABLE(0, 4.3, 15000, 8.00, 0.5, wave)
   WAVETABLE(0.4, 5.2, 15000, 8.07, 0.5, wave)
```

  
  
fun stuff\!

```cpp
   rtsetparams(44100, 2)
   load("WAVETABLE")

   reset(20000)

   wave = maketable("wave3", 1000, 1, 1, 0, 2, 0.2, 90.0, 3.5, 0.4, 0)
   amp = 10000
   ampenv = maketable("line", 1000, 0,0, 0.1,1, 0.2,1, 0.3,0)

   start = 0
   for(i = 0; i < 100; i = i+1) {
      freq = irand(200, 700)
      WAVETABLE(start, 0.3, amp*ampenv, freq, random(), wave)
      start = start + irand(0.01, 0.1)
   }
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[pchcps](../scorefile/pchcps.html), [AMINST](AMINST.html),
[FMINST](FMINST.html), [HALFWAVE](HALFWAVE.html),
[MULTIWAVE](MULTIWAVE.html), [SYNC](SYNC.html), [VWAVE](VWAVE.html),
[WAVESHAPE](WAVESHAPE.html), [WAVY](WAVY.html), [WIGGLE](WIGGLE.html)
