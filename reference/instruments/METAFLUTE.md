---
title: METAFLUTE()
layout: ref
---

## METAFLUTE

Physical model flute.

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**SFLUTE**(outskip, dur, noiseamp, length1 (samples), length2 (samples),
amp\[, pan\])  
  
  
**VSFLUTE**(outskip, dur, noiseamp, length1low (samples), length1high
(samples), length2low (samples), length2high (samples), amp, vibfreq1low
(Hz), vibfreq1high (Hz), vibfreq2low (Hz), vibfreq2high (Hz)\[, pan\])  
  
  
**BSFLUTE**(outskip, dur, noiseamp, length1low (samples), length1high
(samples), length2low (samples), length2high (samples), amp\[, pan\])  
  
  
**LSFLUTE**(outskip, dur, noiseamp, length1 (samples), length2
(samples), amp\[, pan\])  

-----

  
**METAFLUTE** is a family of instruments implmenting a physical model of
a flute. The different versions allow for pitch-bending (**BSFLUTE**,
vibrato (**VSFLUTE**), and an 'unarticulated' (legato) attack
(**LSFLUTE** -- **SFLUTE** is the basic model).  
  
  
<span id="SFLUTE"></span> **SFLUTE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | noise amplitude | relative to overall amplitude; usually 0-1 | no | no | 
p3 | length1 | samples, usually 5-200 | no | no | 
p4 | length2 | samples, usually 5-200 | no | no | 
p5 | amplitude multiplier | relative multiplier of input signal | no | no | 
p6 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   Assumes function table 1 is the noise amplitude envelope
   function table 2 is the overall amplitude envelope
 
   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Brad Garton

  
<span id="VSFLUTE"></span> **VSFLUTE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | noise amplitude | relative to overall amplitude; usually 0-1 | no | no | 
p3 | length1 low value | samples, usually 5-200 | no | no | 
p4 | length1 high value | samples, usually 5-200 | no | no | 
p5 | length2 low value | samples, usually 5-200 | no | no | 
p6 | length2 high value | samples, usually 5-200 | no | no | 
p7 | amplitude multiplier | relative multiplier of input signal | no | no | 
p8 | vibrato frequency 1 low value | Hz, usually < 20 | no | no | 
p9 | vibrato frequency 1 high value | Hz, usually < 20 | no | no | 
p10 | vibrato frequency 2 low value | Hz, usually < 20 | no | no | 
p11 | vibrato frequency 2 high value | Hz, usually < 20 | no | no | 
p12 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   assumes function table 1 is the noise amplitude envelope
   function table 2 is the overall amplitude envelope,
   function table 3 is the vibrato waveform for length1 (between p3 and p4),
   function table 4 is the vibrato waveform for length2 (between p5 and p6)

   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Brad Garton

  
<span id="BSFLUTE"></span> **BSFLUTE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | noise amplitude | relative to overall amplitude; usually 0-1 | no | no | 
p3 | length1 low value | samples, usually 5-200 | no | no | 
p4 | length1 high value | samples, usually 5-200 | no | no | 
p5 | length2 low value | samples, usually 5-200 | no | no | 
p6 | length2 high value | samples, usually 5-200 | no | no | 
p7 | amplitude multiplier | relative multiplier of input signal | no | no | 
p8 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   assumes function table 1 is the noise amplitude envelope
   function table 2 is the overall amplitude envelope,
   function table 3 is the control envelope for length1 (between p3 and p4),
   function table 4 is the control envelope for length2 (between p5 and p6)

   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Brad Garton

  
<span id="LSFLUTE"></span> **LSFLUTE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time | seconds | no | no | 
p1 | duration | seconds | no | no | 
p2 | noise amplitude | relative to overall amplitude; usually 0-1 | no | no | 
p3 | length1 | samples, usually 5-200 | no | no | 
p4 | length2 | samples, usually 5-200 | no | no | 
p5 | amplitude multiplier | relative multiplier of input signal | no | no | 
p6 | pan | 0-1 stereo; 0.5 is middle | no | yes | default: 0 | 

   Because this instrument has not been updated for pfield control,
   the older makegen control envelope sysystem should be used:

   assumes function table 1 is the noise amplitude envelope
   function table 2 is the overall amplitude envelope
 
   Parameters after the [bracket] are optional and default to 0 unless
   otherwise noted.

   Author: Brad Garton

  
**METAFLUTE** is a physical modeling instrument based on Perry Cook's
original "Slide Flute" waveguide model. **METAFLUTE** has several
variants: a simple flute model (**SFLUTE**), a model with pitch-bending
(**BSFLUTE**), and a model with wavetable-defined vibrato (**VSFLUTE**).
These could be consolidated into a single instrument with pfield
control. Hasn't happened yet, but probably should, because there really
isn't an equivalent in the STK instruments
([MBLOWBOTL](MBLOWBOTL.html), etc.).

**LSFLUTE** does the same thing as **SFLUTE**, but without
reinitializing the internal buffers, allowing you to get legato and
slurred-note effects -- just like a REAL flute\!

### Usage Notes

The **METAFLUTE** instruments are older, reflected in the use of the old
[makegen](../scorefile/makegen.html) control envelope and waveform table
system as well as the use of different instruments to accomplish tasks
(pitch bending, vibrato) that could easily be handled via pfield
controls. The modeled flutes are also tricky to use; the tuning is
dependent on the length of the slides and the 'context' of the note when
it is played.

Specifying pitch via two slide lengths isn't optimum, but that's the way
the model was constructed. (remember this was one of the first ones
Perry did) Here is a rough tuning-table for length1 and length2 to
produce a given note:

```cpp
  c8 == middle "C", 8.00 in oct.pc

  note   length1   length2
   g7      112       25
   ab7     106       24
   a7      100       19
   bb7     95        21
   b7      89        19
   c8      84        18
   db8     79        23
   d8      75        19
   eb8     70        15
   e8      67        16
   f8      63        19
   gb8     59        17
   g8      56        17
   ab8     53        25
   a8      50        16
   bb8     47        12
   b8      44        11
   c9      42        10
   db9     39        10
   d9      37        8
   eb9     35        9
   e9      33        18
   f9      31        18
   gb9     29        39
   g9      28        14
   ab9     26        15
   a9      24        17
   bb9     23        14
   b9      22        34
   c10     21        9
```

These are approximate (in fact, they were done "by ear"), so your
mileage may vary.

**LSFLUTE** will not work if more than one **SFLUTE** is active. An
**SFLUTE** note has to precede it, although it can have 0 duration. To
be honest, **LSFLUTE** isn't all that effectve (and I'm not sure it
functions properly).

### Sample Scores

**SFLUTE**:

```cpp
   rtsetparams(44100, 2)
   load("METAFLUTE")

   makegen(1, 24, 1000, 0,1, 1.5,1)
   makegen(2, 24, 1000, 0,0, 0.05,1, 1.49,1, 1.5,0)
   SFLUTE(0, 1.5, 0.1, 40, 28, 7000)
   SFLUTE(1.5, 1.5, 0.1, 40, 22, 7000)
```

  
  
**VSFLUTE**:

```cpp
   rtsetparams(44100, 2)
   load("METAFLUTE")
   
   makegen(1, 7, 1000, 1, 1000, 1)
   makegen(2, 7, 1000, 1, 1000, 1)
   makegen(3, 10, 1000, 1)
   makegen(4, 10, 1000, 1)
   VSFLUTE(0, 3.5, 0.1, 60,62, 30,40, 5000, 1.0,4.0, 1.0,5.0)
   VSFLUTE(4, 3.5, 0.1, 48,50, 30,45, 5000, 4.0,7.0, 3.0,5.0)
```

  
  
**BSFLUTE**:

```cpp
   rtsetparams(44100, 2)
   load("METAFLUTE")

   makegen(1, 7, 1000, 1, 1000, 1)
   makegen(2, 7, 1000, 1, 1000, 1)
   makegen(3, 7, 1000, 0, 500, 1, 500, 0)
   makegen(4, 7, 1000, 0, 1000, 1)
   BSFLUTE(0, 1.5, 0.1, 40,50, 28,40, 5000)
```

  
  
**LSFLUTE**:

```cpp
   rtsetparams(44100, 2) 
   load("METAFLUTE")
   
   makegen(1, 24, 1000, 0, 1, 1.5, 1)
   makegen(2, 24, 1000, 0, 1, 1.5, 1)
   SFLUTE(0.000000, 0.100000, 0.050000, 100.000000, 100.000000, 5000.000000)
   LSFLUTE(0.000000, 0.280000, 0.040000, 50.000000, 16.000000, 5000.000000)
   LSFLUTE(0.280000, 0.280000, 0.040000, 50.000000, 26.000000, 5000.000000)
   LSFLUTE(0.560000, 0.280000, 0.040000, 20.000000, 46.000000, 5000.000000)
   LSFLUTE(0.840000, 0.280000, 0.040000, 50.000000, 16.000000, 5000.000000)
```

  

-----

### See Also

[makegen](../scorefile/makegen.html), [CLAR](CLAR.html),
[MBLOWBOTL](MBLOWBOTL.html), [MBLOWHOLE](MBLOWHOLE.html),
[MBRASS](MBRASS.html), [MCLAR](MCLAR.html), [MSAXOFONY](MSAXOFONY.html)
