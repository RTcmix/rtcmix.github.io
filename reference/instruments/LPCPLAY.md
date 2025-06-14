---
title: LPCPLAY/LPCIN()
layout: ref
---

## LPCPLAY / LPCIN

Linear-predictive filter coding resynthesis.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**LPCPLAY**(outsk, dur, AMP, PITCH, startframe, endframe\[, WARP,
RESONCF, RESONBW\])  
  
**LPCIN**(outsk, insk, dur, AMP, startframe, endframe\[, inchan, WARP, RESONCF,
RESONBW\])  
  
**dataset**(name \[, npoles\])  
  
**lpcstuff**(thresh, noiseamp \[, unvoicedrate, rise, decay,
threshcutoff\])  
  
**set\_thresh**(voiced\_thresh, unvoiced\_thresh)  
  
**setdev**(dev)  

**setdevfactor**(devfactor)  

**set\_hnfactor**(factor)  
    
**freset**(param)  

**use\_autocorrect**(flag)

##### Experimental:

**fix\_pitch_octaves**(flag)

**fix\_pitch\_gaps**(flag)

**pitch\_smoothing**(flag)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.

The subcommands of **LPCPLAY** are used to set various parameters for
the LPC synthesis. Several of them necessarily need to precede the
**LPCPLAY** command itself. See the *Usage Notes* below.  
  
For all commands, parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

<span id="LPCPLAY"></span> **LPCPLAY**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | amplitude multiplier | relative multiplier of original signal | yes | no | 
p3 | transposition | see Usage Notes | yes | no | 
p4 | starting LPC frame |  -  | no | no | 
p5 | ending LPC frame |  -  | no | no | 
p6 | warp factor | -1.0 - 1.0 | yes | yes | default: 0 | 
p7 | reson center frequency | Hz  | yes | yes | value of 0 bypasses filter | 
p8 | reson bandwidth | multiple of center freq | yes | yes | used only if p7 is specified | 

  
<span id="LPCIN"></span> **LPCIN**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | input start time | seconds | no | no | 
p2 | duration | seconds | no | no | 
p3 | amplitude multiplier | relative multiplier of original signal | yes | no | 
p4 | starting LPC frame |  -  | no | no | 
p5 | ending LPC frame |  -  | no | no | 
p6 | input channel |  -  | no | yes | audio channel from input file to read
p7 | warp factor | -1.0 - 1.0 | yes | yes | default: 0 | 
p8 | reson center frequency | Hz  | yes | yes | value of 0 bypasses filter | 
p9 | reson bandwidth | multiple of center freq | yes | yes | used only if p8 is specified | 
  
<span id="dataset"></span> **dataset**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | path to  LPC analysis data file | valid path | no | no | 
p1 | number of filter poles in the original analysis | 0-64 | no | yes | the default value 0 will cause the number of filter poles to be read from the analysis file]

   NOTE: this subcommand is required for LPCPLAY to function

  
<span id="lpcstuff"></span> **lpcstuff**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | voice/unvoiced threshold | usually <= 0.1 for normal resynthesis | no | no | 
p1 | noise amplitude | usually <= 0.1 for normal resynthesis | no | no | 
p2 | unvoiced frame rate |  -  | no | yes | the default value 0 will cause voiced and unvoiced frames to be synthesized at the same rate
p3 | rise | seconds  | no | yes | default: 0; applies internal gain ramp-up
p4 | decay | seconds | no | yes | default: 0; applies internal gain ramp-down
p5 | threshold cutoff |  -  | no | yes | default 0 | frame amplitudes below this value will be muted

Though p0 will work to set the voices/unvoiced threshold, it is advisable the **set_thresh** subcommand instead.

NOTE: this subcommand is required for **LPCPLAY** to function

  
<span id="set_thresh"></span> **set\_thresh**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | voiced | buzz | no | no | threshold (usually close to 0.1 | 
p1 | unvoiced | noise | no | no | threshold (usually close to 0.1 also | 

   NOTE: this subcommand is optional for **LPCPLAY** to function

  
<span id="set_hnfactor"></span> **set\_hnfactor**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | harmonic count in buzz (voiced signal) | no | no | should be > 0 | 

   NOTE: this subcommand is optional for **LPCPLAY** to function

<span id="setdevfactor"></span> **setdev**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | amount of pitch deviation to allow | Hz | no | no | default of 0 disables

   NOTE: this subcommand is optional for **LPCPLAY** to function

<span id="setdevfactor"></span> **setdevfactor**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | percentage of original pitch deviation |  >= 0  | no | no |  default of 0 disables

   NOTE: this subcommand is optional for **LPCPLAY** to function

  
<span id="freset"></span> **freset**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | sets how often the resynthesized signal's pitch is recalculated |  -  | no | no | 

   NOTE: this subcommand is optional for **LPCPLAY** to function

  
<span id="use_autocorrect"></span> **use\_autocorrect**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | 0 or 1: turns automatic unstable-frame filter correction on or off | no | no | 

   NOTE: this subcommand is optional for **LPCPLAY** to function

  

-----

  
**LPCPLAY** is one of the oldest and least-updated instruments in the
RTcmix distribution. It is a fairly complex instrument, and it requires
a number of steps (and "massaging" of data) for it to function properly.
It is a fairly specific synthesis technique (called "source-filter" or
"formant" synthesis, using a real-world model; typically a human voice
or an instrument with pronounced filter formants), and it also has a
relatively unique sound. Paul Lansky used LPC for many of his early
'breakthrough' computer-music pieces, such as *Six Fantasies on a Poem
by Thomas Campion* and his *Idle Chatter* pieces.

### Usage Notes

To use **LPCPLAY**, you first need to create an LPC analysis. The best
program for doing this is Doug Scott's
[MiXViews](https://music.columbia.edu/~doug/MixViews/MiXViews.html) app.
MiXViews extracts the filter (formant) information and does a pitch
analysis on your source (or model) file. The resulting file (using the
ancient term "dataset") is then read by the **LPCPLAY** instrument and
used to guide the sound synthesis.

There are many parameters that can be set, and many of them interact
with each other. Here are a few of the effects possible:

  - The frame rate of the original analysis is typically 200-300
    frames/second \[NOTE: This is set in the original analysis; i.e. by
    MixViews\]. By altering the startframe and endframe relative to the
    duration specified in **LPCPLAY**, time-stretching or
    time-compression of the resynthesized sound will occur.  
  - The **set\_thresh** subcommand determines when the resynthesis
    algorithm will use a pitched (buzz) sound source or an unpitched
    (noise) sound source to send through the filters. This is intended
    to allow for the synthesis of vowels (pitched) and consonants
    (unpitched). The determination is made by checking the error value
    of the pitch-tracking algorithm and comparing it to the thresholds
    specified. Setting the voiced\_thresh parameter to a negative number
    and the unvoiced\_thresh to 0.0 will produce a completely unvoiced
    ("whispered") output. Setting them both to a value \> 1.0 (the
    unvoiced\_thresh has to be \> the unvoiced\_thresh) will result in
    completely 'voiced' speech.  
  - The pitch/transposition parameter (p3) in **LPCPLAY** can be used in many
    different ways.  In the discussion below, *base frequency* is the calculated
    average of all the frame frequencies in the specified LPC data frames.
    - **p3 > -1.0 && p3 < 1.0:** Pitch transposed by p3 semitones (may be fractional)
    - **p3 > 20.0:** Pitch transposed by ratio of p3 (Hz) to base frequency (Hz)
    - **p3 > 0.0 && p3 <= 20.0:** Pitch transposed by ratio of p3 (pch) to base frequency (Hz)
    - **p3 < -20.0:** Pitch set to flat value of -p3 (Hz)
    - **p3 >= -20 && p3 < -1.0:** Pitch set to flat value of -p3 (pch)

  - **setdev** controls how much in Hertz an adjusted pitch analysis will
    deviate from its *base frequency* -- a value of 1 will produce
    almost no spread. Higher values will start reflecting more and more of
    the pitch deviation from the original pitch analysis. 0 turns this
    action off.  
  - The warp parameter will shift the filter formants up or down from
    their original values. \> 0.0 causes an upwards shift, \< 0.0 does a
    downward shift.
  - The three experimental utilities, **fix\_pitch_octaves**, **fix\_pitch\_gaps**, and **pitch\_smoothing**, attempt to auto-correct the pitch frame data in the LPC analysis prior to performing resynthesis.  Each is just a flag to turn the procedure on or off.  All are off by default.

**LPCIN** functions the same way as **LPCPLAY**, except that an input
soundfile takes the place of the pitched (buzz) signal in the
resynthesis. The various threshold parameters then determine when this
input sound will be sent through the filters. The result is a
vocoder-like sound. It is usually wise to choose a source sound with a
wide frequency range represented, as the LPC filters subtract quite a
bit from the signal.

Care should be taken in the original analysis to fix unstable filter
frames (or turn on **use\_autocorrect**) and edit the pitch tracking data for best results.

NOTE: **LPCPLAY** and **LPCIN** are mono-output instruments.
NOTE: For calls to **LPCIN**,  p[6] is the input channel to read, so the warp factor is in p[7], etc.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   dataset("myanalysisfile.lpc")

   lpcstuff(0.09, 0.1)
   set_thresh(0.09, 0.1)

   // basic resynthesis, frames 1-890 for a duration of 4.5 seconds
   LPCPLAY(0, 4.5, 1.0, 0.0, 1, 890)
```

  
  
another basic one:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   dataset("myanalysisfile.lpc")

   lpcstuff(0.09, 0.1)
   set_thresh(-1.0, 0.0) // unvoiced ("whispering") resynthesis

   // stretch the time by a factor of 2, still using frames 1-890
   LPCPLAY(0, 2*4.5, 1.0, 0.0, 1, 890)
```

  
  
an LPCIN example:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   rtinput("mysoundfile.aif")

   dataset("myanalysisfile.lpc")

   lpcstuff(0.09, 0.1)
   set_thresh(0.09, 0.1)

   LPCIN(0, 0.0, 4.5, 1.0, 0.0, 1, 890, inchan=0)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   dataset("myanalysisfile.lpc")

   lpcstuff(0.09, 0.1)
   set_thresh(0.09, 0.1)

   // time-stretch and build a floating chord
   LPCPLAY(0, 14.5, 1.0, 0.0, 1, 890)
   LPCPLAY(0, 14.5, 1.0, -0.02, 1, 890)
   LPCPLAY(0, 14.5, 1.0, 0.05, 1, 890)
   LPCPLAY(0, 14.5, 1.0, 0.07, 1, 890)
```

  
  
another slightly more advanced:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   dataset("myanalysisfile.lpc")

   lpcstuff(0.09, 0.1)
   set_thresh(0.09, 0.1)

   setdev(1) // monotone pitch, virtually no deviation
   // warp formants down, synthesis at middle "C"
   LPCPLAY(0, 4.5, 1.0, 8.00, 1, 890, -0.3)
   // warp formants up, synthesis at "G" below middle "C"
   LPCPLAY(2.1, 4.5, 1.0, 7.07, 1, 890, 0.213)
```

  
  
an older score, showing various aspects:

```cpp
   rtsetparams(44100, 1)
   load("LPCPLAY")

   dataset("myanalysisfile.lpc")

   lpcstuff(thresh = .09,  randamp = .1,   0, 0,0,0)
   set_thresh(buzthresh = 0.09, noisethresh = 0.1);

   fps = 44100/250

   frame1=0
   frame2=600
   warp=0
   bw=0
   cf=0
   amp=10

   /* this calculation is just a trick to make 'dur' exactly equal to the */
   /* time elapsed between frame1 and frame2 of the lpc data */
   dur=(frame2-frame1)/fps

   /* straightforward synthesis */
   LPCPLAY(start=0,dur,amp,transp = .00001,frame1,frame2,warp,cf,bw)

   setdev(1)  /* very slight deviation about base pitch, flat result */
   LPCPLAY(start=start+dur+1,dur,amp,transp = 8,frame1,frame2,warp,cf,bw)

   setdev(0)  /* back to normal deviation, slower, higher, raise formants */
   LPCPLAY(start=start+dur+1,dur*1.5,amp,transp = .08,frame1,frame2,warp=.2,cf,bw)

   /* lower, slower, lower formants --sex change operation */
   LPCPLAY(start=start+dur*1.5+1,dur*1.5,amp,transp= -.12,frame1,frame2,warp=-.25,cf,bw)

   /* even more */
   LPCPLAY(start=start+dur*1.5+1,dur*1.5,amp,transp= 6.00,frame1,frame2,warp=-.25,cf,bw)

   /* distorted curve, some formant shift, speeding up slightly */
   setdev(30)
   LPCPLAY(start=start+dur*1.5+1,dur*.9,amp,transp=.02,frame1,frame2,warp=-.1,cf,bw)

   /* modify pitch curves */
   setdev(0)
   LPCPLAY(start=start+dur+1,dur*.9,amp,transp=8,frame1,frame2,warp=0,cf, bw,frame1+50,8,frame1+100,7,frame1+150,7.05,frame2,9)

   /* some whispered speech */
   lpcstuff(thresh = -.01, randamp = .1,   0,0,0,0)
   set_thresh(0.9, 1);
   LPCPLAY(start=start+dur+1,dur,amp,transp=8,frame1,frame2,warp=0,cf,bw)

   /* highpass whispered speech */
   LPCPLAY(start=start+dur+1,dur,amp,transp=8,frame1,frame2,warp=0,cf=5,bw=.1)

   /* highpass whispered speech, shift formants */
   LPCPLAY(start=start+dur+1,dur,amp,transp=8,frame1,frame2,warp=-.3,cf=7,bw=.05)

   /* andrews sisters */
   lpcstuff(thresh = .09,  randamp = .1,   0, 0,0,0)
   set_thresh(buzthresh, noisethresh);
   setdev(15)
   amp = 3
   LPCPLAY(start=start+dur+1,dur,amp,transp=.01,frame1,frame2,warp=0,cf=0,bw=0)
   LPCPLAY(start,dur,amp,transp=.05,frame1,frame2,warp=0,cf=0,bw=0)
   LPCPLAY(start,dur,amp,transp=.08,frame1,frame2,warp=0,cf=0,bw=0)
```


-----

### See Also

[CONVOLVE1](CONVOLVE1.html), [EQ](EQ.html), [FIR](FIR.html),
[FILTERBANK](FILTERBANK.html), [IIR](IIR.html), [JFIR](JFIR.html),
[LPCIN](LPCIN.html), [VOCODE2](VOCODE2.html), [VOCODE3](VOCODE3.html),
[VOCODESYNTH](VOCODESYNTH.html)
