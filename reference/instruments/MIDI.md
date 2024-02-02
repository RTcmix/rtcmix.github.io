---
title: MIDI()
layout: ref
---

## MIDI

Real-time, scheduled commands for sending MIDI events to external devices.

*in RTcmix/insts/doug*  
  
-----

##### quick syntax:

**setup\_midi()**

**PROGRAM**(event\_time, event\_dur, midi\_chan, program\_number)

**controller_number**(controller\_name)

**CONTROLLER**(event\_time, event\_dur, midi\_chan, controller\_number, CONTROLLER\_VALUE)

**NOTE**(event\_time, event\_dur, midi\_chan, pitch, velocity)

**PITCHBEND**(event\_time, event\_dur, midi\_channel, BEND\_VALUE)

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands).

<span id="setup_midi"></span> **setup\_midi**  

No parameters.

NOTE: this subcommand is required for the **MIDI** commands to function

<span id="PROGRAM"></span> **PROGRAM**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0	| event time | seconds | no | no |
p1	| event duration | seconds | no | no | no effect for **PROGRAM** command
p2	| midi channel | 0-15 | no | no |
p3 | program number | integer, 0-16383 | no | no |


<span id="controller_number"></span> **controller_number**(controller\_name)

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0	| controller name | "bank select", "modulation", "breath controller", etc. | no | no |

NOTE: this subcommand is optional for the **MIDI** commands to function


<span id="CONTROLLER"></span> **CONTROLLER**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0	| event time | seconds | no | no |
p1	| event duration | seconds | no | no | for controller curves
p2	| midi channel | 0-15 | no | no |
p3 | controller number | integer, 0-127 | no | no | 
p4 | controller value | 0-1.0 (normalized) | yes | no | 

<span id="NOTE"></span> **NOTE**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0	| event time | seconds | no | no |
p1	| event duration | seconds | no | no | note-off automatic after duration passes
p2	| midi channel | 0-15 | no | no |
p3 | midi note pitch | oct.pc | no | no | note that pitch is *not* note number
p3 | midi note velocity | 0-1.0 (normalized) | no | no | 

<span id="PITCHBEND"></span> **PITCHBEND**  

Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0	| event time | seconds | no | no |
p1	| event duration | seconds | no | no | 
p2	| midi channel | 0-15 | no | no |
p3 | pitch bend value | -1.0 - 1.0 (normalized) | yes | no |

Parameters labled as Dynamic can receive dynamic updates from a table or real-time control source.

Author: Doug Scott

-----

The **MIDI** family of commands allow delivery of MIDI events to an external MIDI device
at times which are synchronized with internally-generated audio events.

### Usage Notes

Two setup operations are needed to use these instruments.

* [set_option](../scorefile/set_option.html) must be called to set the *midi\_outdevice*.  The value should be a string containing the name of the MIDI device on your computer/phone/etc. that you want your events delivered to.  (See the score examples below).

* **setup\_midi** initializes the **RTcmix** MIDI output code.  After that, any combination of the commands may be called.

NOTE: The continuous-range MIDI values such as note velocity and pitch bend are normalized to fit between -1.0 or 0.0 and 1.0.

### Sample Scores

a simple scale

```cpp
set_option("midi_outdevice = Internal MIDI: Bus 2");
rtsetparams(44100, 2, 1024);
load("MIDI");

setup_midi();

start = 0;
dur = 1/4;
chan = 0;
pitch = 7.00;
vel = 0.5;
incr = 0.01;

for (n = 0; n < 8; ++n) {
        NOTE(start, dur, chan, pitch, vel);
        start += dur;
        pitch += incr;
        vel += 0.05;
}
```
sending program and controller changes

```cpp
set_option("midi_outdevice = Internal MIDI: Bus 2");
rtsetparams(44100, 2, 1024);
load("MIDI");

setup_midi();

// This returns the MIDI controller number for the expression controller

expr = controller_number("expression");

// We will use a table curve to generate a smooth controller change over the
// duration of the note

exprvolume = maketable("line", "nonorm", 1000, 0, 1, 1, 0.1, 2, 1);

start = 0;
dur = 5;
chan = 0;
pitch = 8.00;
vel = 0.8;

PROGRAM(start, 0, 1, chan, 23); // load accordian

CONTROLLER(start, dur, chan, expr, exprvolume);
NOTE(start, dur, chan, pitch, vel);
```

pitch bent accordion (if attached to a General MIDI synth)

```cpp
set_option("midi_outdevice = Internal MIDI: Bus 2");
rtsetparams(44100, 2, 1024);
load("MIDI");

setup_midi();

// We will use a table curve to generate a smooth pitch bend change

bendcurve = maketable("line", "nonorm", 1000, 0, 0, 1, 1, 2, -1, 3, 0);

start = 0;
dur = 5;
chan = 0;
pitch = 8.00;
vel = 0.8;

PROGRAM(start, 0, 1, chan, 23); // load accordian

NOTE(start, dur, chan, pitch, vel);
PITCHBEND(start, dur, chan, bendcurve);
```

-----

### See Also

[set\_option](../scorefile/set_option.html)
