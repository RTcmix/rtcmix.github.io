---
title: MMODALBAR()
layout: ref
---

## MMODALBAR

Modal-bar physical model.

*in RTcmix/insts/stk*  
  

-----

##### quick syntax:

**MMODALBAR**(outsk, dur, AMP, FREQ, hardness, pos, preset\[, PAN,
AMPENV\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3 = frequency (Hz)
   p4 = stick hardness (0.0-1.0)
   p5 = stick position (0.0-1.0)
   p6 = modal preset
      - Marimba = 0
      - Vibraphone = 1
      - Agogo = 2
      - Wood1 = 3
      - Reso = 4
      - Wood2 = 5
      - Beats = 6
      - Two Fixed = 7
      - Clump = 8
   p7 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0.5]
   p8 = amplitude envelope table [optional; default is 1.0]


   p2 (amplitude), p3 (frequency) and p7 (pan) can receive dynamic updates from
   a table or real-time control source.

   p8 (amplitude envelope table), if used, should be a reference to a pfield table-handle.

   Author:  Brad Garton, based on code from the Synthesis ToolKit
```

  

-----

  
**MMODALBAR** is the "ModalBar" physical model in Perry Cook and Gary
Scavone's [STK](http://www.cs.princeton.edu/~prc/NewWork.php#STK), the
Synthesis ToolKit.

### Usage Notes

**MMODALBAR** uses waveguide physical modeling techniques to reproduce
the sound of a range of 'struck bar' instruments, including marimba,
vibraphone, agogo-bells, wood-blocks, etc.

Here's what Perry says about "ModalBar":

and this about the "Modal" class:

The amplitude envelope table (p8, "AMPENV") is an optional parameter. It
is also redundant -- the same functionality can be achieved by using a
pfield-table reference in p2 ("AMP").

See also the [MBANDEDWG](MBANDEDWG.html) instrument for an extended
modal-model implementation.

**MMODALBAR** can produce other mono or stereo output.

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1)
   load("MMODALBAR")

   MMODALBAR(0, 1.0, 25000, 243.0, 0.4, 0.4, 0)
   MMODALBAR(1, 1.0, 25000, 243.0, 0.4, 0.4, 1)
   MMODALBAR(2, 1.0, 25000, 243.0, 0.4, 0.4, 2)
   MMODALBAR(3, 1.0, 25000, 243.0, 0.4, 0.4, 3)
   MMODALBAR(4, 1.0, 25000, 243.0, 0.4, 0.4, 4)
   MMODALBAR(5, 1.0, 25000, 243.0, 0.4, 0.4, 5)
   MMODALBAR(6, 1.0, 25000, 243.0, 0.4, 0.4, 6)
   MMODALBAR(7, 1.0, 25000, 243.0, 0.4, 0.4, 7)
   MMODALBAR(8, 1.0, 25000, 243.0, 0.4, 0.4, 8)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 2)
   load("MMODALBAR")

   st = 0
   for (j = 0; j < 9; j = j+1)
   {
      hard = 0.0
      for (i = 0; i < 10; i = i+1)
      {
         MMODALBAR(st, 1.0, 25000, 279.0, hard, 0.4, j)
         hard = hard + 0.1
         st = st + 0.5
      }
      MMODALBAR(st, 1.0, 25000, 243.0, hard-0.01, 0.4, j)
   }
```

  
  
fun stuff\!

``` 
   rtsetparams(44100, 2)
   load("MMODALBAR")

   amp = 20000
   ampenv = maketable("line", 1000, 0,1, 1, 1, 1.1, 0)
   pches = { 7.05, 7.07, 7.09, 7.10, 8.00, 8.03, 8.05, 8.07, 8.08, 8.09, 8.10, 9.00, 9.05, 9.07, 10.00 }
   lpches = len(pches)

   st = 0
   for (i = 0; i < 200; i = i+1)
   {
      pos = random()
      hard = random()
      inst = trand(0, 9)
      pch = pches[trand(0, lpches)]
      MMODALBAR(st, 1.0, amp*ampenv, cpspch(pch), hard, pos, inst)
      st = st + 0.11
   }
```

  

-----

### See Also

[AMINST](AMINST.html), [FMINST](FMINST.html),
[MBANDEDWG](MBANDEDWG.html), [MBOWED](MBOWED.html),
[MMESH2D](MMESH2D.html), [MSHAKERS](MSHAKERS.html), [STRUM](STRUM.html),
[STRUM2](STRUM2.html), [STRUMFB](STRUMFB.html), [WIGGLE](WIGGLE.html)
