---
title: A Short Tour of PField Capabilities
layout: ref
---

# A Short Tour of PField Capabilities

The *PField* control mechanism, introduced in RTcmix v. 4.0, is a way to
control aspects of Instruments as notes are executing. What the heck
does that mean? Well, instrument parameters that are "pfield-enabled"
can receive and respond to data as a note is being synthesized. In
previous versions of RTcmix, the parameters a note had when it was
scheduled was the parameters it got. Period. Using PField control, note
parameters can be changed dynamically. This is accomplished using
special variables called *pfield-handles* or *table-handles*.

These two terms are used somewhat interchangeably because they both
accomplish pretty much the same thing, and they both allow pretty much
the same operations. There is a difference, however, in that
*table-handles* generally deal with data that is fixed in advance (such
as wavetables), although potentially modifiable during sound synthesis.
*pfield-handles* are usually variables through which data generated
"on-the-fly" will be sent. For the purposes of this 'short tour', we
won't really differentiate between the two.

The best way to get a handle on these *handles* is to see how they are
used. Suppose that we have what we think is an almost perfect sound:

```cpp
rtsetparams(44100, 2)
load("WAVETABLE")

amp = maketable("window", 1000, "hanning")
wavetable = maketable("wave", 1000, "tri")

WAVETABLE(0, 3.5, 20000*amp, 7.05, 0.5, wavetable)
```

We are using two *table-handles* already: *amp* and *wavetable*.
However, this is only an <u>almost</u> perfect sound. Our unerring
internal aesthetic sensibility tells us that it would be
<u>absolutely</u> perfect if only we could add a little vibrato to the
note.

There are two ways to do this. The HARD WAY is to [rewrite the
instrument](instrumentdesign.html) to accommodate another oscillator to
do the vibrato-ing. The EASY WAY is to notice in the
[WAVETABLE](../reference/instruments/WAVETABLE.html) documentation that
p3 (the pitch) is 'pfield-enabled'. This means that we can dynamically
change the pitch of the note as it plays. Hey\! That's what vibrato is\!

How can we generate a vibrato pitch deviation to use in WAVETABLE p3?
The pfield-generating scorefile command
[makeLFO](../reference/scorefile/makeLFO.html) is what we need ("LFO"
stands for **L**ow **F**requency **O**scillator).

We are specifying the pitch of our almost-perfect sound using
octave.pitch-class notation (7.05). In this notation, one semitone on
the equal-tempered scale is equivalent to 0.01. If we want our vibrato
to deviate from the base pitch by one-half semitone above and below the
pitch, then we need to generate an LFO signal that will travel between
-0.005 and +0.005, and then add this to our base pitch. *makeLFO* makes
this trivial to do:

```cpp
vibsig = makeLFO("sine", 1.5, -0.005, 0.005)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig, 0.5, wavetable)
```

The pfield-handle variable *vibsig* will track a sine waveform that
cycles at 1.5 Hz, travelling between -0.005 and +0.005. Adding this to
the base pitch of 7.05 will give us a sound that modulates between 7.045
and 7.055. Our perfect sound\!

One caution about the above code -- the *vibsig* deviating between
-0.005 and +0.005 will work fine for an oct.pitch-class of 7.05, but be
wary of what would happen if our base pitch was 8.00 (think about it).
Often a vibrato is better done using a direct frequency (Hz)
specification.

Noticing that other p-fields of WAVETABLE are listed as
"pfield-enabled", it might be fun to extend our real-time control. Let's
imagine that we want to randomly pan our vibrato-ed note between the two
output channels. A handy *pfield-handle* command called
[makerandom](../reference/scorefile/makerandom.html) seems like it would
do the job nicely:

```cpp
pan = makerandom("linear", 2.0, 0.0, 1.0)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig, pan, wavetable)
```

The problem is that *makerandom* command above generates discrete values
2.0 times/second, and this causes discontinuous shifts in the signal
between the two channels (i.e. clicks). We need a way of smoothing the
signal coming through the *pan* variable. The *pfield-handle* command
*makefilter* can accomplish this:

```cpp
pan = makerandom("linear", 2.0, 0.0, 1.0)
smoothpan = makefilter(pan, "smooth", 70)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig, smoothpan, wavetable)
```

This vibrato-ed sound is so wonderful that <u>TWO</u> vibrato-ed notes
will obviously double our musical pleasure. But we want to be
artistically engaging about our sonic productions, so we decide that we
want one note to vibrato in exactly the opposite direction (pitch-wise,
that is) as the other. We can also use the *makefilter* command to do
this:

```cpp
vibsig = makeLFO("sine", 1.5, -0.005, 0.005)
pan = makerandom("linear", 2.0, 0.0, 1.0)
smoothpan = makefilter(pan, "smooth", 70)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig, smoothpan, wavetable)

vibsig2 = makefilter(vibsig, "invert", 0.0)
pan2 = makerandom("linear", 2.0, 0.0, 1.0)
smoothpan2 = makefilter(pan2, "smooth", 70)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig2, smoothpan2, wavetable)
```

Note the use of separate *pan/pan2* and *smoothpan/smoothpan2* variables
for the two WAVETABLE notes. This is so that the two notes would have
independent panning trajectories. Be aware that in general most
*pfield-handle* variables are assumed to be assigned to only one
executing note. At present (RTcmix 4.0) this is part of how the PField
system works; the *pfield-handle* variables 'draw' data into the note,
and it is assumed that each one will be relatively unique. You can,
however, reuse *pfield-handle* variable names in the scorefile, so that
if you wanted to have two WAVETABLE notes with vibrato operating with
the same LFO frequency, you could do the following:

```cpp
vibsig = makeLFO("sine", 1.5, -0.005, 0.005)
WAVETABLE(0, 3.5, 20000*amp, 7.05+vibsig, 0.0, wavetable)
vibsig = makeLFO("sine", 1.5, -0.005, 0.005)
WAVETABLE(0, 3.5, 20000*amp, 7.02+vibsig, 1.0, wavetable)
```

But *table-handles* don't have this restriction, and *IN THE FUTURE*
this will probably be lifted for every use of *pfield-* and
*table-handles*.

As a final modification to our incredibly amazing vibrato sound, let's
suppose that we want to show off our highly-trained abilities to move a
mouse/cursor to control the amplitude of our notes. Instead of assigning
the *amp* variable to a *table-handle* (*maketable*), we can do this:

```cpp
amp = makeconnection("mouse", "x", minval=0.0, maxval=1.0, default=0.5, lag=80)
```

and the 0.0-1.0 amplitude range will be determined by the mouse/cursor
position along the x-axis in a window that will be created when the
scorefile executes. We can use this variable in both instances of
WAVETABLE because the data is being read continuously from the mouse
position.

So our final, <u>*Absolutely Amazing and Wonderful*</u> scorefile is:

```cpp
rtsetparams(44100, 2)
load("WAVETABLE")

amp = makeconnection("mouse", "x", 0.0, 1.0, 0.5, 80)
wavetable = maketable("wave", 1000, "tri")

vibsig = makeLFO("sine", 1.5, -0.005, 0.005)
pan = makerandom("linear", 2.0, 0.0, 1.0)
smoothpan = makefilter(pan, "smooth", 70)
WAVETABLE(0, 35.0, 20000*amp, 7.05+vibsig, smoothpan, wavetable)

amp2 = makeconnection("mouse", "x", 0.0, 1.0, 0.5, 80)
vibsig2 = makefilter(vibsig, "invert", 0.0)
pan2 = makerandom("linear", 2.0, 0.0, 1.0)
smoothpan2 = makefilter(pan2, "smooth", 70)
WAVETABLE(0, 35.0, 20000*amp2, 7.05+vibsig2, smoothpan2, wavetable)
```

We have increased the duration to 35.0 seconds because it is just so
much fun to play around with the amplitude using the mouse. We also
might have reused the variables *amp, vibsig, pan* and *smoothpan* in
the second WAVETABLE note, but decided to give them separate names for
the heck of it.

## Useful PField Scorefile Commands

The following, in no particular order, are scorefile commands that can
be used to create and manipulate *pfield-handles* and *table-handles*:

  - [maketable](../reference/scorefile/maketable.html) -- the general command for creating 'tables', or arrays of values that can be used as control functions, waveforms, etc.
  - [modtable](../reference/scorefile/modtable.html) -- this command can be used to modify the data from from a *table-handle*.
  - [copytable](../reference/scorefile/copytable.html) -- this makes a copy of a table from a *table-handle* variable. Very useful for guaranteeing that you have 'fixed' data... many of the *table-handle* and *pfield-handle* commands operate on data 'on the fly', or as it is being generated. *copytable* can be used to increase execution efficiency, also.
  - [tablelen](../reference/scorefile/tablelen.html) -- returns the length (number of elements) of a table from a *table-handle*.
  - [add](../reference/scorefile/add.html), [div](../reference/scorefile/div.html), [mul](../reference/scorefile/mul.html), [sub](../reference/scorefile/sub.html) -- arithmetic operations that work on all the data in a table given a *table-handle* variable.

#### pfield-handle commands
      
  - [makeLFO](../reference/scorefile/makeLFO.html) -- generate Low Frequency Oscillation (usually \< 20 Hz) data and feed it through a *pfield-handle* variable.
  - [makerandom](../reference/scorefile/makerandom.html) -- periodically generate some type of random number and feed it through a *pfield-handle* variable.
  - [makeconnection](../reference/scorefile/makeconnection.html) -- establish a connection to an 'outside' data source and feed it through a *pfield-handle* variable.
  - [makefilter](../reference/scorefile/makefilter.html) -- alter the data coming through a *pfield-handle* variable in some way and feed the result through another *pfield-handle* variable.
  - [makeconverter](../reference/scorefile/makeconverter.html) -- apply a data conversion operation like [octcps](../reference/scorefile/octcps.html), [pchmidi](../reference/scorefile/pchmidi.html), [ampdb](../reference/scorefile/ampdb.html), etc., to the data coming through a *pfield-handle* variable feed the result through another *pfield-handle* variable.

      
#### data output and display commands
      
  - [samptable](../reference/scorefile/samptable.html) -- return a value from a table given a *table-handle*.
  - [makemonitor](../reference/scorefile/makemonitor.html) -- display or record data coming through a *pfield-handle* variable.
  - [plottable](../reference/scorefile/plottable.html) -- plot the data in a table from a *table-handle*.
  - [dumptable](../reference/scorefile/dumptable.html) -- print out the contents of a table given a *table-handle*.

  
  
Brad Garton  
