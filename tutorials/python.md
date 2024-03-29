---
title: Using Python as the RTcmix Command-Language Interface
layout: ref
---

# Using Python as the *RTcmix* Command-Language Interface

Very similar to the [Perl](perl.html) front-end procedure for RTcmix,
you will need to configure RTcmix using the *--with-python*
configuration flag (see the [installation guide](../standalone/index.html)
for information about this). Once you do this and (re-)compile RTcmix,
you're set to go with a Python front-end. As with Perl, this can be used
in place of the default scorefile language
[Minc](../reference/scorefile/Minc.html). You will need to use the
[PYCMIX](../reference/standalone/CMIX.html) command instead of
[CMIX](../reference/standalone/CMIX.html) to start RTcmix.

## Using Python/RTcmix

To use RTcmix within PYCMIX, you must import the rtcmix extension like this:

```python
from rtcmix import *
```

    or (preferably)

```python
import rtcmix as rt
```

In the latter case, use the "rt." prefix for all RTcmix functions. E.g., 

```python
rt.rtsetparams(44100, 2)
rt.load("WAVETABLE")
```


## Sample Scorefiles

Here is a Python script showing the RTcmix ability to chain several
instruments together. (This is a translation of
"sample_scores/LONGCHAIN_1.sco from Minc to Python.")

```python
# This score makes a wavetable synth riff and feeds it through 3 effects
# in series: flange -> delay -> reverb
    
from rtcmix import *
    
print_off()
rtsetparams(44100, 2)
load("WAVETABLE")
load("FLANGE")
load("JDELAY")
load("REVERBIT")
    
bus_config("WAVETABLE", "aux 0-1 out")
bus_config("FLANGE", "aux 0-1 in", "aux 10-11 out")
bus_config("JDELAY", "aux 10-11 in", "aux 4-5 out")
bus_config("REVERBIT", "aux 4-5 in", "out 0-1")
    
totdur = 30
masteramp = 1.0
atk = 2; dcy = 4
    
notes = [5.00, 5.001, 5.02, 5.03, 5.05, 5.07, 5.069, 5.10, 6.00]
numnotes = len(notes)
    
transposition = 2.00    # try 7.00 also, for some cool aliasing...
srand(2)
    
# ----------------------------------------------------------- synth ---
notedur = .10
incr = notedur + .015
    
maxampdb = 92
minampdb = 75
    
control_rate(20000)         # need high control rate for short synth notes
env = maketable("line", 10000, 0,0, 1,1, 20,0)
wavet = maketable("wave", 10000, 1, .9, .7, .5, .3, .2, .1, .05, .02)
    
ampdiff = maxampdb - minampdb
st = 0
while st < totdur:
   slot = int(random() * numnotes * .999999)
   pitch = pchoct(octpch(notes[slot]) + octpch(transposition))
   amp = ampdb(minampdb + (ampdiff * random()))
   pan = random()
   WAVETABLE(st, notedur, amp * env, pitch, pan, wavet)
   st += incr
    
# for the rest
control_rate(500)
amp = masteramp
    
# ---------------------------------------------------------- flange ---
resonance = 0.3
lowpitch = 5.00
moddepth = 90
modspeed = 0.08
wetdrymix = 0.5
flangetype = 0
    
wavetabsize = 100000
wavet = maketable("wave", wavetabsize, "sine")
    
maxdelay = 1.0 / cpspch(lowpitch)
    
setline(0,1,1,1)
    
st = 0; insk = 0; inchan = 0; pan = 1; ringdur = 0
FLANGE(st, insk, totdur, amp, resonance, maxdelay, moddepth,
       modspeed, wetdrymix, flangetype, inchan, pan, ringdur, wavet)
    
lowpitch += 0.07
maxdelay = 1.0 / cpspch(lowpitch)
    
wavet = maketable("wave3", wavetabsize, 1, 1, -180)
    
st = 0; insk = 0; inchan = 1; pan = 0; ringdur = 0
FLANGE(st, insk, totdur, amp, resonance, maxdelay, moddepth,
       modspeed, wetdrymix, flangetype, inchan, pan, ringdur, wavet)
    
    
# ----------------------------------------------------------- delay ---
deltime = notedur * 2.2
regen = 0.70
wetdry = 0.12
cutoff = 0
ringdur = 2.0
    
env = maketable("line", 1000, 0,0, atk,1, totdur-dcy,1, totdur,0)
    
st = 0; insk = 0; inchan = 0; pan = 1
JDELAY(st, insk, totdur, amp * env, deltime, regen, ringdur, cutoff,
       wetdry, inchan, pan)
st = 0.02; inchan = 1; pan = 0
JDELAY(st, insk, totdur, amp * env, deltime, regen, ringdur, cutoff,
       wetdry, inchan, pan)
    
# ---------------------------------------------------------- reverb ---
revtime = 1.0
revpct = 0.3
rtchandel = 0.05
cf = 0
    
st = 0; insk = 0
REVERBIT(st, insk, totdur+ringdur, amp, revtime, revpct, rtchandel, cf)

``` 
