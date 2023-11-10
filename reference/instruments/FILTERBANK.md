---
title: FILTERBANK()
layout: ref
---

## FILTERBANK

Multi-band reson instrument.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**FILTERBANK**(outsk, insk, dur, AMP, ringdowndur, inputchan, PAN,
CFREQ1, BANDWIDTH1, RELAMP1, ... CFREQN, BANDWITHN, RELAMPN)

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
p2 | input duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of input signal | yes | no | 
p4 | ring-down duration | seconds | no | no | 
p5 | input channel |  -  | no | no | 
p6 | pan | 0-1 stereo; 0.5 is middle | yes | no | 
p7 | center freq for filter | Hz or oct.pc* | yes | no | see note below
p8 | bandwidth for filter | multiplier of the center frequency, 0-1 | yes | no |
p9 | relative amplitude for filter in final construction | - | yes | no |
... |

The remaining pfields are triples following the format of p7-p9.  Up to 60 cf-bw-amp triples can be specified.

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

\* If the value of the center frequency pfield(s) ("CFREQ1 ... CFREQN") is < 15.0,
it assumes oct.pc.  Use the pchcps
scorefile convertor for direct frequency specification below 15.0 Hz.

Author: John Gibson, 25 Feb 2007

  

-----

  
**FILTERBANK** is very similar to the older [IIR](IIR.html) instrument.
It builds a series of infinite impulse response (or recursive) filters
with center frequency, bandwidth, and amplitude boost defined in
triplets. These filters, combined together, allow the specification of
complex frequency-response curves.

### Usage Notes

The two main differences between **FILTERBANK** and [IIR](IIR.html) are,
first, the elimination of the **setup** subcommand for designing the
filter in favor of extended pfield-parameters in the main **FILTERBANK**
note specification. This leads to the second main difference: each of
the center-frequency/bandwith/relative-amplitude may be dynamically
modified using the [pfield-enabled](pfield-enabled.html) control
system. This gives the **FILTERBANK** instrument a high degree of
flexibility.

oct.pc format generally will not work as you expect for the center
frequency specifications if the pfield changes dynamically. This is
because of the 'mod 12' aspect of the pitch-class (.pc) specification.
Use direct frequency (hz) or linear octaves instead.

The bandwith of each filter is specified as a multiplier of the center
frequency of each filter. The older [IIR](IIR.html) instrument had an
option to specify this bandwidth directly as a Hz value. This is not the
case with **FILTERBANK**.

The point of the ring-down duration parameter (p4) is to let you control
how long the filter will sound after the input has stopped. Too short a
time, and the sound may be cut off prematurely, resulting in a click.

The output of **FILTERBANK** can be either mono or stereo.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("FILTERBANK")
   
   rtinput("mysound.aif")
   inchan = 0
   inskip = 0
   dur = DUR()
   ringdur = 10
   
   amp = 0.25
   bw = 0.0003
   
   cf1 = maketable("line", "nonorm", 100, 0,200, 1,1200)
   cf2 = maketable("line", "nonorm", 100, 0,1100, 1,300)
   cf3 = maketable("line", "nonorm", 100, 0,600, 1,2200)
   cf4 = maketable("line", "nonorm", 100, 0,2000, 1,900)
   
   start = 0
   FILTERBANK(start, inskip, dur, amp, ringdur, inchan, pan=0.5,
      cf1, bw, g=1, cf2, bw, g=1, cf3, bw, g=1, cf4, bw, g=1)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   load("FILTERBANK")
   
   rtinput("mysound.aif")
   inchan = 0
   inskip = 0
   dur = DUR()
   ringdur = 5
   
   amp = 0.1
   bw = maketable("line", "nonorm", 1000, 0,0.01, dur,0.001, dur+ringdur,0.001)
   
   start = 0
   FILTERBANK(start, inskip, dur, amp, ringdur, inchan, pan=0.1,
      cf=9.02, bw, g=1,
      cf=9.09, bw, g=1,
      cf=10.01, bw, g=1,
      cf=10.06, bw, g=1,
      cf=11.02, bw, g=1)
   FILTERBANK(start, inskip, dur, amp, ringdur, inchan, pan=0.9,
      cf=8.02, bw, g=1,
      cf=9.05, bw, g=1,
      cf=10.03, bw, g=1,
      cf=10.10, bw, g=1,
      cf=11.04, bw, g=1)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [BUTTER](BUTTER.html),
[ELL](ELL.html), [EQ](EQ.html), [FIR](FIR.html),
[FILTSWEEP](FILTSWEEP.html), [FOLLOWBUTTER](FOLLOWBUTTER.html),
[IIR](IIR.html), [JFIR](JFIR.html), [MOOGVCF](MOOGVCF.html),
[MULTEQ](MULTEQ.html) [SPECTEQ](SPECTEQ.html), [SPECTEQ2](SPECTEQ2.html)
