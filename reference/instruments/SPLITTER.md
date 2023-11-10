---
title: SPLITTER()
layout: ref
---

## SPLITTER

Send mono input to multiple outputs.

*in RTcmix/insts/jg*  
  

-----

##### quick syntax:

**SPLITTER**(outsk, insk, dur, GLOBAL\_AMP, input\_channel,
OUTCHAN0\_AMP, OUTCHAN1\_AMP, ...)

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
p3 | global amp multiplier | relative multiplier of input signal | yes | no | 
p4 | input channel |  -  | no | no | 
p5-pN | amplitude multiplier for each output channel | relative multiplier | yes | no | 

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

John Gibson, 1/22/06

  

-----

  
**SPLITTER** is a utility istrument for routing signals into the
different RTcmix busses (and outputs).

### Usage Notes

**SPLITTER** does essentially the opposite that the [MIX](MIX.html)
instrument does, with an added amplitude multiplier for each output
channel in the output matrix. The number of outputs is set by the
[bus\_config](../scorefile/bus_config.html) scorefile command.

**SPLITTER** is necessary for setting up parallel processing chains,
where the signal originating from one instrument or input is intended to
be routed through several seperate processing chains simultaneously.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 2)
   
   load("WAVETABLE")
   load("SPLITTER")
   
   dur = 10
   amp = 5000
   freq = 440
   bus_config("WAVETABLE", "aux 0 out")
   WAVETABLE(0, dur, amp, freq)
   bus_config("WAVETABLE", "aux 1 out")
   freq = 770
   WAVETABLE(0, dur, amp, freq)
   
   bus_config("SPLITTER", "aux 0 in", "aux 10 out", "aux 12 out")
   inchan = 0
   SPLITTER(0, 0, dur, 1, inchan, 1, 1)
   bus_config("SPLITTER", "aux 1 in", "aux 11 out", "aux 13 out")
   SPLITTER(0, 0, dur, 1, inchan, 1, 1)
   
   //bus_config("MIX", "aux 10-11 in", "out 0-1")
   bus_config("MIX", "aux 12-13 in", "out 0-1")
   MIX(0, 0, dur, 1, 0, 1)
```

  
  
slightly more advanced:

```cpp
   rtsetparams(44100, 2)
   
   load("STRUM2")
   load("FLANGE")
   load("PANECHO")
   load("SPLITTER")
   
   bus_config("STRUM2", "aux 0-1 out")
   bus_config("SPLITTER", "aux 0-1 in", "aux 2-3 out", "aux 4-5 out")
   bus_config("MIX", "aux 2-3 in", "out 0-1")
   bus_config("FLANGE", "aux 4-5 in", "aux 6-7 out")
   bus_config("PANECHO", "aux 6-7 in", "out 0-1")
   
   srand()
   
   beat = 1
   bases = { 10.00, 9.07, 9.05, 9.00 }
   
   amp = 30000
   
   notes = { 0.00, 0.02, 0.04, 0.05, 0.07, 0.09, 0.11, 1.00 }
   nnotes = len(notes)
   
   durs = {  1,    1,    1,    1,    2,    2,    2,    2,
      1,    1,    1,    1,    2,    2,    4,
      1,    1,    1,    1,    2,    2,    2,    2,
      1,    1,    1,    1,    2,    2,    4 }
   ndurs = len(durs)
   
   tdur = 0 
   for (i = 0; i < ndurs; i += 1) tdur += durs[i]
   tdur *= beat
   // do the whole thing 2X
   tdur *= 2
   
   
   hitincr = 0.05
   dur = beat*0.3
   st = irand(0, 7*beat)
   while (st < tdur) {
    nhits = irand(150, 250)
    base = bases[trand(0, len(bases))]
    for (i = 0; i < nhits; i += 1) {
        pitch = base + notes[trand(0, nnotes)]
        sq = 10 - ((i/nhits) * 8)
        STRUM2(st, dur, amp, pitch, sq, dur, random())
        st += irand(0, hitincr)
    }
   
    for (i = 0; i < nhits; i += 1) {
        pitch = base + notes[trand(0, nnotes)]
        sq = 8 * (i/nhits) + 2
        STRUM2(st, dur, amp, pitch, sq, dur, random())
        st += irand(0, hitincr)
    }
   
    st += irand(5*beat, 11*beat)
   }
   
   
   // flange + panecho
   SPLITTER(0, 0, st, 1, 0, 1.0, 0, 1.8, 0)
   SPLITTER(0, 0, st, 1, 1, 0, 1.0, 0, 1.8)
   
   fwave = maketable("wave", 10000, "sine")
   
   amp = 0.8
   c0del = irand(0.4, 0.7)
   c1del = irand(0.2, 0.45)
   
   FLANGE(0, 0, st, amp, 0.02, 0.005, 70, 0.4, 1, "iir", 0, 0.5, 1.0, fwave)
   
   PANECHO(0, 0, st, 0.7, c0del, c1del, 0.8, 10.0, 0)
   
   MIX(0, 0, st, 1, 0, 1)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html), [MIX](MIX.html),
[NPAN](NPAN.html), [PAN](PAN.html), [QPAN](QPAN.html),
[REVMIX](REVMIX.html), [STEREO](STEREO.html)
