---
title: VOCODE3()
layout: ref
---

## VOCODE3

Channel vocoder.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**VOCODE3**(outsk, insk, dur, AMP, MODCFREQS, CARCFREQS, BANDMAP,
CAMPSCALE, MODCFTRANSP, CARCFTRANSP, MODFILTQ, CARFILTQ\[, FILTRESPONSE,
HOLD, PAN\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable-2.html) or
[makeconnection](../scorefile/makeconnection-2.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

-----

  

``` 
   p0 = output start time (seconds)
   p1 = input start time (seconds)
   p2 = duration (seconds)
   p3 = amplitude multiplier (relative multiplier of output signal)
   p4 = table of modulator center frequencies (Hz or linear octaves) 
   p5 = table of carrier center frequencies (Hz or linear octaves)
   p6 = table mapping modulator to carrier filter bands (0 == no mapping)
   p7 = carrier amplitude scaling table (0 == no mapping)
   p8 = transposition of modulator center frequencies (linear octaves)
   p9 = transposition of carrier center frequencies (linear octaves)
   p10 = modulator filter Q (i.e., cf / bandwidth in Hz)
   p11 = carrier filter Q
   p12 = filter response time (seconds) [optional; default is 0.01]
   p13 = hold -- 0: normal, 1: modulator is disengaged and has no further
      effect on the carrier.  [optional; default is 0]
   p14 = pan (0-1 stereo; 0.5 is middle) [optional; default is 0.5]

   p3 (amplitude), p8 (modulator center freq transposition), p9 (carrier center freq transposition),
   p10 (modulator filter Q), p11 (carrier filter Q), p12 (filter response time), p13 (hold) and
   p14 (pan) can receive dynamic updates from a table or real-time control source.

   p4 (modulator center frequencies), p5 (carrier center frequencies),
   p6 (if used, modulator to carrier filter band mapping) and
   p7 (if used, carrier amplitude scaling) should be references to pfield table-handles.

   Author:  John Gibson, 6/3/02
```

  

-----

  
**VOCODE3** performs a filter-bank analysis of the right input channel
(the modulator), and uses the time-varying energy measured in the filter
bands to control a corresponding filter bank that processes the left
input channel (the carrier). You can configure independently the two
filter banks, and you can map modulator bands to carrier bands freely.

### Usage Notes

The carrier/modulator approach used in **VOCODE3** is highly similar to
the scheme used in the older [VOCODE2](VOCODE2.html) instrument as well
as the amplitude-envelope following instrument
[FOLLOWER](FOLLOWER.html). Currently in RTcmix it's not possible for an
instrument to take input from both an "in" bus and an "aux in" bus at
the same time. So, for example, if you want the modulator to come from a
microphone, which must enter via an "in" bus, and the carrier to come
from a [WAVETABLE](WAVETABLE.html) instrument via an "aux" bus, then you
must route the mic into the [MIX](MIX.html) instrument as a way to
convert it from "in" to "aux in". If you want the carrier to come from a
file, then it must first go through [MIX](MIX.html) (or some other
instrument) to send it into an aux bus. Since the instrument is usually
taking input from an aux bus, the input start time for this instrument
must be zero. The only exception would be if you're taking the carrier
and modulator signals from the left and right channels of the same sound
file.

The "left" input channel comes from the bus with the lower number; the
"right" input channel from the bus with the higher number.

This instrument is similar in some respects to [PVOC](PVOC.html), but it
is a channel vocoder using a bank of band-pass filters instead of an FFT
analysis (like with phase vocoders). This kind of instrument was
originally designed for cross-synthesis work, but a wide range of
effects are possible.

Parameters p4 ("MODCFREQS") and p5 ("CARCFREQS") are pfield references
to tables giving center frequencies for the bandpass filters. The tables
must be the same size; the size determines the number of filters. You
can pass the same table to both pfields. Try using the "literal" variant
of [maketable](../scorefile/maketable.html#literal) to create the
tables.

The table mapping modulator to carrier filter bands (p6, "BANDMAP") must
be the same size as the two center frequency tables. The indices of the
table represent modulator bands; the values of the table represent
carrier bands. So a table of { 2, 3, 0, 1 } connects the 0th modulator
band to the 2nd carrier band, the 1st mod. to the 3rd carrier, the 2nd
modulator to the 0th carrier, and the 3rd modulator to the 1st carrier.
Note that more than one modulator band may map to the same carrier band,
and that (in this case) a carrier band may have no modulator input. Use
"0" to get the default linear mapping: 0-\>0, 1-\>1, 2-\>2, 3-\>3, etc

The carrier amplitude scaling table (p7, "CAMPSCALE") must be the same
size as the table of carrier center frequencies. Each element is a
linear amplitude scaling factor applied to the corresponding carrier
band output.

Parameters p8 ("MODCFTRANSP") and p9 ("CARCFTRANSP") transpose the
modulator center frequencies and the carrier center frequencies
respectively. This is specified using linear octaves (see the [format
conversion routines](../scorefile/ampdb.php/index.html) for scorefile
conversion to different pitch specifiers).

p10 ("MODFILTQ") and p11 ("CARFILTQ") control the 'sharpness' of the
filter bands used for the modulator and the carrier.

The output of **VOCODE3** can be either mono or stereo.

### Sample Scores

one example:

``` 
   rtsetparams(44100, 2, 256)
   load("VOCODE3")

   // CPU load: c. 70% running as root (without COMPLIMIT)
   // with Obalance, it's 40% !!  Must be conversion to floats and inline next()

   totdur = 60

   rtinput("mysound.wav")
   rtinput("mysound2.aiff")

   // modulator
   bus_config("MIX", "in 0", "aux 1 out")
   dur = DUR()
   amp = 1
   for (st = 0; st < totdur; st += dur)
      MIX(st, 0, dur, amp, 0)
   totdur = st

   // carrier
   noise = 0
   if (noise) {
      load("NOISE")
      load("EQ")
      bus_config("NOISE", "aux 2 out")
      bus_config("EQ", "aux 2 in", "aux 0 out")
      amp = 15000
      NOISE(0, totdur, amp, 0)
      EQ(0, 0, totdur, 1, "lowpass", 0, 0, bypass=0, cf=4000, q=0.5)
   }
   else {
      load("WAVETABLE")
      bus_config("WAVETABLE", "aux 0 out")
      amp = 10000
      pitch = 8.00
      //wavet = maketable("wave", 20000, "buzz")
      wavet = maketable("random", 40, "even", 0, 1, seed=1)
      WAVETABLE(0, totdur, amp, pitch, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 0.02, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 0.021, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 0.05, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 0.07, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 0.069, 0, wavet)
      WAVETABLE(0, totdur, amp, pitch + 1.00, 0, wavet)
   }


   // --------------------------------------------------------------------------
   bus_config("VOCODE3", "aux 0-1 in", "aux 4-5 out")

   env = maketable("line", 1000, 0,0, .1,1, totdur-.1,1, totdur,0)

   if (1) {
      numfilts = 64
      numfilts = 32
      tab = {}
      oct = octpch(6.00)
      incr = octpch(0.025)
      for (i = 0; i < numfilts; i += 1) {
         tab[i] = oct
         oct += incr
      }
      modcf = maketable("literal", "nonorm", 0, tab)
   }
   else {
      modcf = maketable("literal", "nonorm", 0,
         octpch(7.00),
         octpch(7.05),
         octpch(7.07),
         octpch(8.00),
         octpch(8.02),
         octpch(8.06),
         octpch(8.09),
         octpch(9.00),
         octpch(9.04),
         octpch(9.11),
         octpch(10.06),
         octpch(11.01)
      )
   }
   numfilts = tablelen(modcf)

   if (1) {
      index = makeconnection("mouse", "x", min=0, max=numfilts, df=0, lag=50, "idx", "", 1)
      //value = makeconnection("mouse", "y", min=5, max=10, df=min, lag=50, "val")
      //modcf = modtable(modcf, "draw", "literal", index, value)

      index = makerandom("even", lfreq=2, min=0, max=1, seed=1)
      //index = makefilter(index, "smooth", lag=0)
      value = makerandom("even", lfreq=.5, min=7, max=12, seed=1)
      value = makefilter(value, "smooth", lag=80)
      //value = makemonitor(value, "display", "value")
      modcf = modtable(modcf, "draw", index, value)
   }

   carcf = modcf

   if (1) {
      maplist = {}
      spray_init(1, numfilts, seed=1)
      for (i = 0; i < numfilts; i += 1)
         maplist[i] = get_spray(1)
      map = maketable("literal", "nonorm", numfilts, maplist)
      dumptable(map)
   }
   else
      map = 0

   scale = 0
   //scale = maketable("curve", "nonorm", numfilts, 0,1,-1, 1,0)
   //scale = maketable("curve", "nonorm", numfilts, 0,0,1, 1,1)

   modtransp = 0.0
   //modtransp = makeconnection("mouse", "x", min=-1, max=3, dflt=0, lag=50, "mtrns")
   cartransp = 1.0
   //cartransp = makeconnection("mouse", "y", min=-2, max=2, dflt=0, lag=50, "ctrns")

   modq = 10.0
   carq = 400.0

   amp = 1.0
   //amp = makeconnection("mouse", "y", min=0, max=1, dflt=0, 0, "amp")

   response = 0.001
   //response = makeconnection("mouse", "x", min=0, max=2, dflt=0.01, 0, "resp")

   hold = 0
   //hold = makeconnection("mouse", "y", min=0, max=2, dflt=0, 0, "hold")
   hold = makeLFO("square2", freq=10, min=0, max=3)
   //hold = makerandom("even", freq=2, min=0, max=1.5, seed=1)

   VOCODE3(0, 0, totdur, amp * env, modcf, carcf, map, scale,
      modtransp, cartransp, modq, carq, response, hold, pan=.9)
   modtransp += 1
   cartransp -= .03
   hold = makeLFO("square2", freq=10, min=0, max=8)
   VOCODE3(0, 0, totdur, amp * env, modcf, carcf, map, scale,
      modtransp, cartransp, modq, carq, response, hold, pan=.1)


   // --------------------------------------------------------------------------
   load("COMPLIMIT")
   ingain = 0
   outgain = 0
   atk = 0.01
   rel = 0.01
   thresh = -1
   ratio = 100
   look = atk
   win = 128
   bus_config("COMPLIMIT", "aux 4 in", "out 0")
   COMPLIMIT(0, 0, totdur, ingain, outgain, atk, rel, thresh, ratio, look, win)
   bus_config("COMPLIMIT", "aux 5 in", "out 1")
   COMPLIMIT(0, 0, totdur, ingain, outgain, atk, rel, thresh, ratio, look, win)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CONVOLVE1](CONVOLVE1.html),
[LPCIN](LPCIN.html), [PVOC](PVOC.html), [SPECTACLE](SPECTACLE.html),
[SPECTACLE2](SPECTACLE2.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html), [SPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODESYNTH](VOCODESYNTH.html)
