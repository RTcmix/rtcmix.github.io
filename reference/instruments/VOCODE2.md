---
title: VOCODE2()
layout: ref
---

## VOCODE2

Channel vocoder.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**VOCODE2**(outsk, insk, dur, AMP, nfilts, CFREQLO/FTABLETRANSP,
CFREQMULT/FILTMULT, transp, filtbw\[, filtresponse, hisigmix, hifreq,
NOISEAMP, noiserate, PAN, CFREQTABLE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of output signal | yes | no | 
p4 | number of filters |  -  | no | no | 
p5 | if p4 > 0, lowest filter center frequency | Hz/oct.pc | yes | no |
   | if p4 == 0, transposition table | (oct.pc)
p6 | if p4 > 0, center frequency spacing multiplier | > 1 | yes | no | 
   | if p4 == 0, multipler of p5 to add additional filters | - |
p7 | amount to transpose carrier filters | Hz/oct.pc | no | no | 
p8 | filter bandwidth proportion of center frequency | > 0 | no | no | 
p9 | filter response time | seconds | no | yes | default: 0.01 | 
p10 | amount of high-passed modulator signal to mix with output | amplitude multiplier | no | yes | default: 0 |
p11 | cutoff frequency for high pass filter applied to modulator. | Hz, ignored if p10 == 0 | yes | no | default: 5000 Hz |
p12 | amount of noise signal to mix into carrier before processing | amplitude multiplier applied to full-scale noise signal | yes | yes | default: 0 |
p13 | specifies how often to get new random values from the noise generator | samples | no | yes | This pfield is ignored if p12 is zero. default is 1 -- a new value every sample |
p14 | pan | 0-1 stereo; 0.5 is middle | yes | yes | default: 0.5 | 
p15 | table giving list of center frequencies | - | yes | yes | if p4 == 0 |

   p3 (amplitude), p12 (noise amp) and p14 (pan) can receive dynamic updates
   from a table or real-time control source.

   p5 (cfreqlo/ftabletransp), p6 (cfreqmult/filtmult) and p15 cfreqtable should be
   references to pfield table-handles if p4 == 0.

   Author:  John Gibson, 6/3/02

  

-----

  
**VOCODE2** performs a filter-bank analysis of the right input channel
(the modulator), and uses the time-varying energy measured in the filter
bands to control a corresponding filter bank that processes the left
input channel (the carrier). The two filter banks have identical
characteristics, but there is a way to shift all of the center
frequencies of the carrier's bank.

The [VOCODE3](VOCODE3.html) instrument may be better to use; it is
fully updated with [pfield-enabled](pfield-enabled.html) parameters.

### Usage Notes

The carrier/modulator approach used in **VOCODE2** is similar to the
amplitude-envelope following instrument [FOLLOWER](FOLLOWER.html).
Currently in RTcmix it's not possible for an instrument to take input
from both an "in" bus and an "aux in" bus at the same time. So, for
example, if you want the modulator to come from a microphone, which must
enter via an "in" bus, and the carrier to come from a
[WAVETABLE](WAVETABLE.html) instrument via an "aux" bus, then you must
route the mic into the [MIX](MIX.html) instrument as a way to convert it
from "in" to "aux in". If you want the carrier to come from a file, then
it must first go through [MIX](MIX.html) (or some other instrument) to
send it into an aux bus. Since the instrument is usually taking input
from an aux bus, the input start time for this instrument must be zero.
The only exception would be if you're taking the carrier and modulator
signals from the left and right channels of the same sound file.

The "left" input channel comes from the bus with the lower number; the
"right" input channel from the bus with the higher number.

This instrument is similar in some respects to [PVOC](PVOC.html), but it
is a channel vocoder using a bank of band-pass filters instead of an FFT
analysis (like with phase vocoders). This kind of instrument was
originally designed for cross-synthesis work, but a wide range of
effects are possible.

If parameter p4 ("nfilts") is \> 0, then p5 ("CFREQLO/FTABLETRANSP") is
interpreted as the lowest center frequency of the filters. This can be
specified in Hz or oct.pc, \< 15.0 is the point at which it will be
interepreted as oct.pc. p6 ("CFREQMULT/FILTMULT") will be interpreted as
a multiplier of p5 -- it will space the center frequencies of all the
filters (up to "nftilts", p4) as a multiple of each succesive center
frequency. For example, p6 = 2.0 will make a stack of octaves; p6 = 1.5
will make a stack of perfect (Pythagorian) fifths. To get stacks of an
equal tempered interval (in oct.pc), you can use the
[cpspch](../scorefile/cpspch.html) convertor:

However, if p4 == 0, then p15 ("CFREQTABLE") has to be present. It is
interpreted as a reference to a table containing the oct.pc center
frequencies of all the filters used. Parameter p5
("CFREQLO/FTABLETRANSP") is a list of values (oct.pc) by which those
filters will be transposed. The number of filters will be dependent upon
the length of this table. p6 ("CFREQMULT/FILTMULT") is interpreted as a
multiplier of the values in the p5 table for easier specification of the
filters. For example, if the table has 300 and 500, and p6 is 2, filters
will be constructed at 300, 500, 600, and 1000 Hz.

Parameters p10 ("hisigmix") and p11 ("hifreq") allow some of the
original signal to appear in the output. This is helpful in capturing
sharp noise transients (such as consonants in speech) that aren't
captured well by standard FFT analysis.

When p13 ("noiserate") is greater than 1 sample, successive noise
samples are connected using linear interpolation. This acts as a
low-pass filter; the higher the interval, the lower the cutoff
frequency.

The output of **VOCODE2** can be either mono or stereo.

### Sample Scores

one example:

```cpp
   rtsetparams(44100, 2)

   load("VOCODE2")
   load("WAVETABLE")

   bus_config("MIX", "in 0", "aux 1 out")
   bus_config("WAVETABLE", "in 0", "aux 0 out")
   bus_config("VOCODE2", "aux 0-1 in", "out 0-1")

   // modulator
   rtinput("mysound.wav")
   inskip = 0
   dur = DUR() - inskip
   amp = 1
   MIX(0, inskip, dur, amp, 0)

   // carrier
   amp = 5000
   wavet = maketable("wave", 10000, "buzz20")
   pitchtab = { 8.00, 8.02, 8.05, 8.07, 8.08, 8.10, 9.00 }
   numpitches = len(pitchtab)
   transp = octpch(0.00)
   for (i = 0; i < numpitches; i += 1) {
      freq = cpsoct(octpch(pitchtab[i]) + transp)
      WAVETABLE(0, dur, amp, freq, 0, wavet)
   }


   // --------------------------------------------------------------------------
   dur += 5
   maxamp = 10.0
   amp = maketable("line", "nonorm", 1000, 0,0, 0.1,maxamp, dur-2,maxamp, dur,0)

   numfilt = 22
   lowcf = 8.07
   interval = 0.025 // oct.pc
   spacemult = cpspch(interval) / cpspch(0)

   cartransp = 0.00
   bw = 0.0002
   resp = 0.02
   hipass = 0.00
   hpcf = 3000
   noise = 0.2
   noisubsamp = 8
   VOCODE2(0, 0, dur, amp, numfilt, lowcf, spacemult, cartransp, bw, resp, hipass, hpcf, noise, noisubsamp, pan=1)

   spacemult += 0.008   // make right channel sound different
   VOCODE2(0, 0, dur, amp, numfilt, lowcf, spacemult, cartransp, bw, resp, hipass, hpcf, noise, noisubsamp, pan=0)
```

  
  
another example:

```cpp
   rtsetparams(44100, 2)
   load("VOCODE2")

   rtinput("mysound.wav")

   // carrier
   bus_config("MIX", "in 0", "aux 0 out")
   inskip = 0
   amp = 1
   dur = DUR() - inskip
   MIX(0, inskip, dur, amp, 0)

   // modulator
   bus_config("MIX", "in 0", "aux 1 out")
   inskip = 0
   dur = DUR() - inskip
   amp = 1
   MIX(0, inskip, dur, amp, 0)

   // --------------------------------------------------------------------------
   bus_config("VOCODE2", "aux 0-1 in", "out 0-1")

   maxamp = 1.5
   amp = maketable("line", "nonorm", 1000, 0,maxamp, dur-1,maxamp, dur,0)

   numfilt = 0 // flag indicating that we're using cftab instead of interval stack

   cftab = maketable("literal", "nonorm", 0,
      7.00,
      7.07,
      8.02,
      8.09,
      9.04,
      9.11,
     10.06,
     11.01
   )
   numpitches = tablelen(cftab)

   transp = 0.07
   freqmult = 2.02
   cartransp = -0.02
   bw = 0.008
   resp = 0.0001
   hipass = 0.1
   hpcf = 5000
   noise = 0.01
   noisubsamp = 4

   dur += 2
   VOCODE2(0, 0, dur, amp, numfilt, transp, freqmult, cartransp, bw, resp, hipass, hpcf, noise, noisubsamp, pan=1, cftab)

   transp = transp + 0.002
   VOCODE2(0, 0, dur, amp, numfilt, transp, freqmult, cartransp, bw, resp, hipass, hpcf, noise, noisubsamp, pan=0, cftab)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CONVOLVE1](CONVOLVE1.html),
[LPCIN](LPCIN.html), [PVOC](PVOC.html), [SPECTACLE](SPECTACLE.html),
[SPECTACLE2](SPECTACLE2.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html), [SPECTACLE](TVSPECTACLE.html),
[VOCODE3](VOCODE3.html), [VOCODESYNTH](VOCODESYNTH.html)
