---
title: WIGGLE()
layout: ref
---

## WIGGLE

Wavetable oscillator with frequency modulation and filter.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**WIGGLE**(outsk, dur, CARAMP, CARFREQ, moddepth, filttype, steepness,
balance, CARWAVE, MODWAVE, MODFREQ, MODDEPTH, LOWPASSFREQ\[, PAN)

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
   p2  = carrier amplitude (absolute, for 16-bit soundfiles: 0-32768)
   p3  = carrier oscillator frequency (Hz/oct.pc)
   p4  = modulator depth control type (0: no modulation at all, 1: percent
         of carrier frequency, 2: modulation index [useful for FM])
   p5  = type of filter (0: no filter, 1: low-pass, 2: high-pass)
   p6  = steepness (> 0)
   p7  = balance output and input signals (0:no, 1:yes)
   p8  = carrier wavetable reference
   p9  = modulator wavetable reference
   p10 = modulator frequency (Hz, or carrier-ratio if negative)
   p11 = modulator depth
   p12 = lowpass filter cutoff frequency (Hz)
   p13 = p5 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0]

   p3 (carrier amplitude), p4 (carrier frequency), p10 (modulator frequency,
   p11 (modulator depth), p12 (low-pass filter cutoff frequency) and p13 (pan)
   can receive dynamic updates from a table or real-time control source.

   p8 (carrier wavetable) and p9 (modulator wavetable) should be references to
   pfield table-handles.

   Author:  John Gibson, 12/4/01; rev. for v4, 6/17/05.
```

  

-----

  
***Older Documentation:***  
  
  

### WIGGLE

wavetable oscillator with frequency modulation and filter (in package
*insts.jg*)  
  
**WIGGLE** is a kind of combination of [WAVETABLE](WAVETABLE_html.html)
and [FMINST](FMINST.html). The time-varying capabilities in the first
version of **WIGGLE** are now possible with the pfield-enabled
[WAVETABLE](WAVETABLE_html.html) and [FMINST](FMINST.html) instruments,
so **WIGGLE** may be redundant. It does do a whole lot of fun synthesis
things, however.

### Usage Notes

Oct.pc format generally will not work as you expect for p3 ("CARFREQ")
if the pfield changes dynamically. Use Hz instead in that case.

The modulator depth control type (p4, "moddepth") tells **WIGGLE** how
to interpret modulator depth values (p11, "MODDEPTH"). You can express
these as a percentage of the carrier (0-100), useful for subaudio rate
modulation, or as a modulation index (useful for audio rate FM). If you
don't want to use the modulating oscillator at all, set p4 to 0.

Steepness (p6 "steepness") is just the number of filters to add in
series. Using more than 1 steepens the slope of the filter. If you don't
set p7 ("balance") to 1, you'll need to change p2 ("CARAMP") to adjust
for loss of power caused by connecting several filters in series.

Parameter p7 ("balance") tries to adjust the output of the filter so
that it has the same power as the input. This means there's less
fiddling around with p2 ("CARAMP") to get the right amplitude when
steepness is \>1. However, it has drawbacks: it can introduce a click at
the start of the sound, it can cause the sound to pump up and down a
bit, and it eats extra CPU time.

Parameter p10 ("MODFREQ") -- the modulator frequency -- is in Hz. Or, if
it is negative, then its absolute value will be interpreted as the
modulator's ratio to the carrier frequency. E.g...

will change gradually from a C:M ratio of 1:2 to a ratio of 1:2.3 over
the course of the note.

The carrier and modulator wavetables can be updated in real time using
[modtable(..., "draw", ...)](../scorefile/modtable.html#draw)

**WIGGLE** can produce either mono or stereo output.

### Sample Scores

*NOTE: These are older scorefiles using the makegen function table
system. This really isn't used much anymore*  
  
  
Sample scorefile 1:

``` 
   rtsetparams(44100, 2)
   load("WIGGLE")

   dur = 12
   amp = 10000
   pitch = 8.00
   mod_depth_type = 1      /* % of car freq */

   setline(0,0, 1,1, 2,1, 5,0)

   makegen(2, 10, 8000, 1,1/2,1/3,1/4,1/5,1/6,1/7,1/8)  /* car waveform */
   makegen(3, 18, 1000, 0,-0.02, 1,0.07)                /* car gliss */
   makegen(4, 10, 8000, 1)                              /* mod waveform */
   makegen(5, 18, 1000, 0,20, 1,3, 2,0)                 /* mod freq */
   makegen(6, 18, 1000, 0,1, 1,20, 2,5)                 /* mod depth */
   makegen(8, 18, 5000, 0,1, 1,.9, 5,0)                 /* pan */

   WIGGLE(st=0.00, dur, amp, pitch, mod_depth_type)

   amp = amp * 0.4
   makegen(2, 10, 8000, 1,0,1/9,0,1/25,0,1/49)          /* car waveform */
   makegen(3, 18, 1000, 0,0.02, 3,-0.10, 4,-2.05)       /* car gliss */
   makegen(5, 18, 1000, 0,21, 1,1, 2,0)                 /* mod freq */
   makegen(6, 18, 1000, 0,1, 1,20, 2,8)                 /* mod depth */
   makegen(8, 18, 5000, 0,0, 2,.1, 5,1)                 /* pan */

   WIGGLE(st=0.05, dur, amp, pitch+2.005, mod_depth_type)
```

  
  
  
Sample scorefile 2:

``` 
   rtsetparams(44100, 2)
   load("WIGGLE")

   dur = 10
   two_layers = 1  /* 0: one layer, 1: two layers */
   amp = 800
   pitch = 11.00

   setline(0,1, dur-.1,1, dur,0)

   makegen(2, 10, 8000, 1,.3,.1)       /* car waveform */

   /* -------------------------------------------------------------------------- */
   /* Use gliss function, created with gen 20, to provide random "vibrato."
      This shows how to compute gen size to get the vibrato rate you want.
      Note that WIGGLE doesn't interpolate between values of the gliss
      function, so you get a clear stair-step effect, like an old analog
      sequencer.  If you want smooth transitions between the steps, use
      the mod oscillator instead.  Then you could also have varying speed,
      which can do with the gliss function.
   */
   vib_speed = 10 /* in Hz */
   dist_type = 0  /* 0=even, 1=low, 2=high, 3=triangle, 4=gaussian, 5=cauchy */
   seed = 1

   /* down as much as a perfect fifth, up as much as an octave */
   min = -0.07    /* in oct.pc */
   max = 1.00

   gen_size = vib_speed * dur

   /* gliss function values are in linear octaves, thus the octpch conversions. */
   makegen(3, 20, gen_size, dist_type, seed, octpch(min), octpch(max))

   WIGGLE(st=0, dur, amp, pitch)

   /* -------------------------------------------------------------------------- */
   if (two_layers) {
      /* same as above, but different seed */
      makegen(3, 20, gen_size, dist_type, seed+1, octpch(min), octpch(max))
      pitch = pitch - 3  /* 3 octaves below */
      WIGGLE(st=0, dur, amp, pitch)
   }
```

  
  
  
Sample scorefile 3:

``` 
   rtsetparams(44100, 2)
   load("WIGGLE")
   load("FREEVERB")
   bus_config("WIGGLE", "aux 0-1 out")
   bus_config("FREEVERB", "aux 0-1 in", "out 0-1")

   dur = 18

   /* --------------------------------------------------------------- wiggle --- */
   amp = 7000
   pitch = 7.00
   mfreq = cpspch(pitch+1)

   setline(0,0, 1,1, 6,1, 8,0)

   makegen(2, 10, 8000, 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1) /* car wave */
   makegen(3, 18, 10, 0,0,1,0)                        /* car gliss */
   makegen(4, 10, 80, 1, 1, .4)                       /* mod wave */
   makegen(5, 18, 20, 0,mfreq, 1,mfreq, 2,mfreq-18)   /* mod freq */
   makegen(6, 18, 2000, 0,1.5, 1,1.8)                 /* mod depth */
   makegen(7, 18, 2000, 0,4000, 1,10000, 2,100)       /* filt cf */
   makegen(8, 18, 2000, 0,0, 1,.5)                    /* pan */

   depth_type = 2  /* mod index */
   filt_type = 1
   filt_steep = 5

   WIGGLE(st=0, dur, amp, pitch, depth_type, filt_type, filt_steep)

   makegen(3, 20, 250, 4, 1, -.04,.04)                /* car gliss */
   makegen(8, 18, 2000, 0,1, 1,0)                     /* pan */
   WIGGLE(st=0.01, dur, amp, pitch, depth_type, filt_type, filt_steep)

   /* --------------------------------------------------------------- reverb --- */
   roomsize = 0.85
   predelay = 0.02
   ringdur = 1.0
   damp = 50
   dry = 80
   wet = 30
   stwid = 100

   FREEVERB(0, 0, dur, amp=1, roomsize, predelay, ringdur, damp, dry, wet, stwid)
```

  
  
  
Sample scorefile 4:

``` 
   rtsetparams(44100, 2)
   load("WIGGLE")

   dur = 25
   amp = 3000
   pitch = 10.00

   makegen(1, 4, 2000, 0,0,2, dur*.1,1,0, dur*.4,1,-3, dur,0)

   makegen(2, 10, 8000, 1)                /* car wave */
   makegen(3, 20, 300, 2, 0, -1.00,2.00)  /* random gliss */

   makegen(4, 10, 1000, 1)                /* mod waveform */
   makegen(5, 18, 20, 0,200, 1,200)       /* mod pitch */
   makegen(6, 18, 2000, 0,20, 1,20)       /* mod depth */

   makegen(-7, 4, 2000, 0,1000,-4, 1,1)   /* filter cf */
   makegen(8, 18, 2000, 0,.2, 1,.5, 2,1)  /* pan */

   filt_type = 2                          /* highpass */
   filt_steep = 20

   WIGGLE(st=0, dur, amp, pitch, 2, filt_type, filt_steep)

   makegen(8, 18, 2000, 0,1, 1,0)         /* pan */
   WIGGLE(st=0.1, dur, amp, pitch+.01, 2, filt_type, filt_steep)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [AMINST](AMINST.html),
[FMINST](FMINST.html), [HALFWAVE](HALFWAVE.html),
[MULTIWAVE](MULTIWAVE.html), [SYNC](SYNC.html), [VWAVE](VWAVE.html),
[WAVESHAPE](WAVESHAPE.html), [WAVY](WAVY.html)
