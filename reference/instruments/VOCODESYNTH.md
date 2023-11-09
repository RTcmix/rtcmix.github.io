---
title: VOCODESYNTH()
layout: ref
---

## VOCODESYNTH

Oscillator-bank channel vocoder.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**VOCODESYNTH**(outsk, insk, dur, AMP, nfilts, CFREQLO/FTABLETRANSP,
CFREQMULT/FTABLEFORMAT, transp, filtbw, windowsize, smoothness,
threshold, attack}, release, hisigmix, hifreq, inputchan, PAN,
WAVETABLE\[, SCALINGTABLE, CFREQTABLE\])

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
p5 | if p4 > 0, lowest filter center frequency | Hz/oct.pc | no | no |
 | if p4 == 0, transposition of function table | oct.pc | 
p6 | if p4 > 0, center frequency spacing multiplier | >1 | yes | no |
 | if p4 == 0, function table format | 0: octave.pc, 1: linear octave; Hz if > 15.0 |
p7 | amount to transpose carrier oscillators | Hz/oct.pc | no | no | 
p8 | filter bandwidth proportion of center frequency | > 0 | no | no | 
p9 | power gauge window length | seconds, try 0.01 | no | no | 
p10 | smoothness -- how much to smooth the power gauge output | 0-1, try 0.5 | no | no | 
p11 | threshold -- below which no synthesis for a band occurs | 0-1, try 0.0 | no | no | 
p12 | attack time -- how long it takes the oscillator for a band to turn on fully once the modulator power for that band rises above the threshold | seconds, try 0.001 | no | no | 
p13 | release time -- how long it takes the oscillator for a band to turn off fully once the modulator power for that band falls below the threshold | seconds, try 0.01] | no | no | 
p14 | amount of high-passed modulator signal to mix with output | amplitude multiplier, try 0.0 | no | no | 
p15 | cutoff frequency for high pass filter applied to modulator | Hz, try 5000 | no | no | This pfield ignored if p10 is zero.  
p16 | input channel |  -  | no | no | 
p17 | pan | 0-1 stereo; 0.5 is middle | yes | no | 
p18 | pfield reference for wavetable to use |  -  | yes | no | 
p19 | pfield reference for table giving the carrier scaling curve |  as [freq, amp] pairs  | yes | no | 
p20 | pfield reference for table giving list of center frequencies | - | yes | no | if p4 is zero

   p3 (amplitude) and p17 (pan) can receive dynamic updates from a table or
   real-time control source.

   p5 (cfreqlo/ftabletransp), p6 (cfreqmult/ftableformat), p18 (wavetable),
   p19 (scalingtable, if used) and p20 (center freq table, if used) should
   be references to pfield table-handles.

   Author:  John Gibson, 8/7/03.

  

-----

  
**VOCODESYNTH** performs a filter-bank analysis of the input channel
(the modulator), and uses the time-varying energy measured in the filter
bands to trigger wavetable notes with corresponding frequencies (the
carrier).

### Usage Notes

This is a bit different from the other VOCODE-family instruments like
[VOCODE2](VOCODE2.html) and [VOCODE3](VOCODE3.html) in that it does a
resynthesis of the signal. This instrument is similar in some respects
to [PVOC](PVOC.html), but it is a channel vocoder using a bank of
band-pass filters instead of an FFT analysis (like with phase vocoders).

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

However, if p4 == 0, then p20 ("CFREQTABLE") has to be present. It is
interpreted as a reference to a table containing the oct.pc center
frequencies of all the filters used. Parametet p6
("CFREQMULT/FTABLEFORMAT") then serves as a flag for how the table
referenced in p20 will be interpreted. If p6 == 0, then the table will
be interpreted as containing oct.pc pitch values. If p6 == 1, then the
table will be interpreted as linear octave values. NOTE: This only
applies if the values in the table ar \< 15.0; otherwise they will be
interpreted as direct frequency values regardless of the setting of p6.
The number of filter bands is determined by length of function table.

An example of this second method for center frequency specification:

  - Make a table containing the desired center frequencies:
    
    ```cpp
       numbands = 5
       cftable - maketable("literal", "nonorm", numbands, 8.00, 8.07, 9.00, 9.07, 10.02)
    ```
    
    or specify the table as direct frequencies:
    
    ```cpp
       numbands = 9
       cftable - maketable("literal", "nonorm", numbands, 100, 200, 300, 400, 500, 600, 700, 800, 900)
    ```

  - Set p4 to 0, and pass the "cftable" pfield-handle in through
    parameter p20. This table will be transposed by the setting given in
    p5 ("CFREQLO/FTABLETRANSP").

Parameter p9 ("windowsize") determines how often changes in modulator
power are measured. p10 ("smoothness") operates upon the output of p9.
It has more of an effect for longer p9 values.

Parameters p14 ("hisigmix") and p15 ("hifreq") allow some of the
original signal to appear in the output. This is helpful in capturing
sharp noise transients (such as consonants in speech) that aren't
captured well by standard FFT analysis. p15 is ignored if p10 is set to
0.

p18 ("WAVETABLE") is a reference to a waveform table that will be used
by the internal wavetable oscillators.

p19 ("SCALINGTABLE") is a table used for scaling the carrier notes,
interpreted as \[frequency, amplitude\] pairs.

**VOCODESYNTH** produces both stereo or mono output.

### Sample Scores

one example:

```cpp
   rtsetparams(44100, 2)
   load("VOCODESYNTH")

   rtinput("mysound.aif")

   inskip = 0.0
   dur = DUR()

   amp = 4.0
   env = maketable("line", 1000, 0,0, .1,1, dur-.1,1, dur,0)

   numbands = 25
   lowcf = 300
   interval = 0.025
   cartransp = 0.00
   bw = 0.009
   winlen = 0.001
   smooth = 0.98
   thresh = 0.0001
   atktime = 0.001
   reltime = 0.01
   hipassmod = 0.0
   hipasscf = 2000

   carwavetable = maketable("wave", 10, 20000, "sine")

   scale1 = 0.5
   scale2 = 1.0
   scalecurve = maketable("curve", "nonorm", 100, 0,scale1,1, 1,scale2)

   spacemult = cpspch(interval) / cpspch(0.0)

   VOCODESYNTH(0, inskip, dur, amp * env, numbands, lowcf, spacemult, cartransp,
      bw, winlen, smooth, thresh, atktime, reltime, hipassmod, hipasscf,
      inchan=0, pan=1, carwavetable, scalecurve)

   cartransp += 0.001
   spacemult += 0.002
   VOCODESYNTH(0, inskip, dur, amp * env, numbands, lowcf, spacemult, cartransp,
      bw, winlen, smooth, thresh, atktime, reltime, hipassmod, hipasscf,
      inchan=0, pan=0, carwavetable, scalecurve)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [CONVOLVE1](CONVOLVE1.html),
[LPCIN](LPCIN.html), [PVOC](PVOC.html), [SPECTACLE](SPECTACLE.html),
[SPECTACLE2](SPECTACLE2.html), [SPECTEQ](SPECTEQ.html),
[SPECTEQ2](SPECTEQ2.html), [SPECTACLE](TVSPECTACLE.html),
[VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html)
