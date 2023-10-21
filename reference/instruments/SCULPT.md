---
title: SCULPT()
layout: ref
---

## SCULPT

Breakpoint oscillator resynthesis.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**SCULPT**(outsk, segmentdur, amp. nsegments\[, pan\])

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = duration of each segment (seconds)
   p2 = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3 = number of points
   p4 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   function table 1 is overall amp envelope
   function table 2 is the synthesis waveform
   function table 3 is a listing of the frequency points
   function table 4 is a listing of the amplitude points

   Author:  Stanko Juzbasic
```

  

-----

  
**SCULPT** is an instrument which does a time-based resynthesis of
frequency / amplitude pairs that have been parsed into
[makegen](../scorefile/makegen.html) function tables.

### Usage Notes

As much fun as this instrument is, its functionality has largely been
superceded by [WAVETABLE](WAVETABLE.html) and
[MULTIWAVE](MULTIWAVE.html) using the pfield control mechanism. Stanko
originally designed it so that output from analysis programs like
IRCAM's [AudioSculpt](http://forumnet.ircam.fr/691.php?L=1) or Michael
Klingbeil's [SPEAR](http://www.klingbeil.com/spear/) (which didn't even
exist when **SCULPT** was written).

If you do use **SCULPT**, be sure not to normalize the values in the
[makegen](../scorefile/makegen.html) tables. Also, be aware that
**SCULPT** does not interpolate between frequency and amplitude values
in the table. Each value holds constant for the length of each segment.

**SCULPT** can produce stereo or mono output.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   load("SCULPT")

   makegen(1, 24, 1000, 0, 1, 1, 1)
   makegen(2, 10, 1000, 1)
   makegen(3, 2, 10, 0)
      149.0 159.0 169.0 179.0 189.0 199.0 214.0 215.0 234.0 314.0
   makegen(4, 2, 10, 0)
      0.0 -7.0 -10.0 -3.0 0.0 -10.0 -20.0 -15.0 -2.1 -1.1

   SCULPT(0, 0.5, 10, 10)
```

  

-----

### See Also

[MULTIWAVE](MULTIWAVE.html), [WAVETABLE](WAVETABLE.html)
