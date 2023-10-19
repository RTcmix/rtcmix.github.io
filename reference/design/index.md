---
title: Instrument Design
layout: ref
---

# Instrument Design

RTcmix provides a set of C/C++ functions and objects for constructing
new signal-processing and synthesis instruments. The source code for
most of these low-level routines are in the "RTcmix/lib" and
"RTcmix/sys" subdirectories of the RTcmix distribution.

In addition to the reference material below, [a tutorial is
available](../../tutorials/instrumentdesign.html) for creating and
compiling RTcmix instruments.

**Arrange by: [Topic](#sort_topic)  · 
[Alphabetical](#sort_alphabetical)**

<span id="topic" style="display:block"> </span>

## Processing Control/Sound Input & Output

  - [Instrument](Instrument.html) -- base class for all RTcmix
    instruments; contains many essential functions and variables
      - [configure](Instrument.html#configure) -- designer-overridden
        virtual function to configure an RTcmix instrument
      - [currentFrame](Instrument.html#currentFrame) -- return current
        sample frame \#
      - [framesToRun](Instrument.html#framesToRun) -- return number of
        frames to run for each *Instrument::run()* invocation
      - [getdur](Instrument.html#getdur) -- returns duration (seconds)
        of the note
      - [getendsamp](Instrument.html#getendsamp) -- returns the
        (absolute) ending sample \# (frame) for a note
      - [getSkip](Instrument.html#getSkip) -- returns the number of
        samples in one control (reset) period
      - [getstart](Instrument.html#getstart) -- returns starting time
        (seconds) of the note
      - [increment](Instrument.html#increment) -- update the
        computed-sample count
      - [init](Instrument.html#init) -- designer-overridden virtual
        function to initialize an RTcmix instrument
      - [inputChannels](Instrument.html#inputChannels) -- return the
        number of input channels
      - [nSamps](Instrument.html#nSamps) -- return total number of
        frames (samples) to run for a note
      - [outputChannels](Instrument.html#outputChannels) -- return the
        number of output channels
      - [run](Instrument.html#run) -- designer-overridden virtual
        function for computing samples
      - [setchunk](Instrument.html#setchunk) -- sets the number of
        sample frames computed for each *Instrument::run()* invocation
      - [setendsamp](Instrument.html#setendsamp) -- sets the (absolute)
        ending sample \# (frame) for a note
  - [Obucket](Obucket.html) -- internally buffer samples or
    floating-point numbers
  - [Ortgetin](Ortgetin.html) -- read input samples from file or input
    device
  - [rtaddout](rtaddout.html) -- send samples to the output device or
    soundfile
  - [rtbaddout](rtbaddout.html) -- send an array of samples to the
    output device or soundfile
  - [rtgetin](rtgetin.html) -- get samples from the input device or
    soundfile
  - [rtinrepos](rtinrepos.html) -- reposition the input read pointer
  - [rtsetinput](rtsetinput.html) -- open and initialize a soundfile or
    input device for reading
  - [rtsetoutput](rtsetoutput.html) -- open and initialize the output
    device and/or soundfile for writing
  - [update](update.html) -- retrieve PField-handle or table-handle data

## Synthesis

  - [Ooscil](Ooscil.html) -- non-interpolating wavetable oscillator
    object; also can be used for control envelopes
  - [Ooscili](Ooscil.html) -- interpolating wavetable oscillator object;
    also can be used for control envelopes
  - [oscil](oscil.html) -- wavetable oscillator
  - [oscili](oscil.html) -- interpolating wavetable oscillator
  - [osciln](oscil.html) -- wavetable oscillator (FM-compatible)
  - [oscilni](oscil.html) -- interpolating wavetable oscillator
    (FM-compatible)
  - [boscili](boscili.html) -- block-computing interpolating wavetable
    oscillator
  - [buzz](buzz.html) -- buzz (pulse) oscillator
  - [bbuzz](buzz.html) -- block-computing buzz (pulse) oscillator
  - [pluckset](pluckset.html) -- simple Karplus-Strong ("plucked
    string") initialization
  - [pluck](pluck.html) -- simple Karplus-Strong ("plucked string")
    filter/generator
  - [bpluck](bpluck.html) -- some sort of Karplus-Strong thing
  - [hplset](hpluck.html) -- Karplus-Strong ("plucked string")
    initialization
  - [hpluck](hpluck.html) -- Karplus-Strong ("plucked string")
    filter/generator
  - [wshape](wshape.html) -- waveshaping function

## Math/Data

  - [ampdb](ampdb.html) -- convert decibels to amplitude
  - [dbamp](ampdb.html) -- convert amplitude to decibels
  - [boost](ampdb.html) -- convert amplitude in decibels to amplitude
    multiplier

### Pitch-specification Conversion Routines

  - [cpsoct](ampdb.html) -- convert linear octaves to Hz
  - [cpspch](ampdb.html) -- convert octave.pitch-class to Hz
  - [midipch](midipch.html) -- convert Hz to midi note \#
  - [octcps](ampdb.html) -- convert Hz to linear octaves
  - [octpch](ampdb.html) -- convert octave.pitch-class to linear octaves
  - [pchcps](ampdb.html) -- convert Hz to octave.pitch-class
  - [pchmidi](pchmidi.html) -- convert midi note \# (byte) to frequency
    (Hz)
  - [pchoct](ampdb.html) -- convert linear octaves to octave.pitch-class

### Random-Number Routines

  - [Orand](Orand.html) -- pseudo-random number generating object
  - [crandom](crandom.html) -- another psuedo-random number generator
  - [randf](randf.html) -- 1/f pseudo-random number generator(?)
  - [srrand](rrand.html) -- pseudo-random number generator
    seed/initialization
  - [rrand](rrand.html) -- pseudo-random number generator
  - [sbrrand](brrand.html) -- block-computed pseudo-random number
    generator seed/initialization
  - [brrand](brrand.html) -- block-computed pseudo-random number
    generator

## Function-table Slot Operations

  - [floc](floc.html) -- return a pointer to a function table slot
  - [fsize](fsize.html) -- return the size of a function table slot
    array
  - [evset](evp.html) -- envelope/control function initialization
  - [evp](evp.html) -- envelope/control function generator
  - [install\_gen](install_gen.html) -- install a function table slot
    from within an instrument
  - [combine\_gens](combine_gens.html) -- combine function tabbles
  - [resample\_gen](resample_gen.html) -- resample a function table
  - [setline](setline.html) -- create line-segment curve in an array
  - [tableset](table.html) -- initialize a function table slot for
    envelopes/control functions
  - [table](table.html) -- read from a function table slot for
    envelopes/control functions
  - [tablei](table.html) -- interpolated read from a function table slot
    for envelopes/control functions

## Filters

  - [Ocomb](Ocomb.html) -- comb filter object
  - [Ocombi](Ocomb.html) -- interpolating comb filter object
  - [Odcblock](Odcblock.html) -- DC blocking filter object
  - [Oequalizer](Oequalizer.html) -- multi-purpose biquad filter object
  - [Oonepole](Oonepole.html) -- simple recursive one-pole filter object
  - [OonepoleTrack](OonepoleTrack.html) -- simple 'tracking' recursive
    one-pole filter object
  - [Oreson](Oreson.html) -- simple recursive bandpass filter object
  - [allpass](allpass.html) -- allpass filter
  - [allpole](allpole.html) -- allpole filter
  - [ballpole](ballpole.html) -- block-computing allpole filter
  - [rsnset](reson.html) -- two-pole FIR filter initialization
  - [reson](reson.html) -- simple two-pole FIR filter
  - [breson](breson.html) -- block-computed two-pole FIR filter
  - [bresonz](bresonz.html) -- block-computed two-pole FIR filter
  - [rszset](resonz.html) -- simple IIR filter initialization
  - [resonz](resonz.html) -- simple IIR filter

## Delays

  - [Odelay](Odelay.html) -- delay line object
  - [Odelayi](Odelay.html) -- interpolating delay line object
  - [combset](allpass.html) -- comb filter setup function
  - [comb](allpass.html) -- comb filter
  - [hcomb](hcomb.html) -- interpolating comb filter
  - [delset](delget.html) -- initialize a delay line
  - [delput](delget.html) -- put a sample into a delay line
  - [delget](delget.html) -- get a sample from a delay line
  - [dliget](delget.html) -- get an interpolated sample from a delay
    line
  - [rvbset](reverb.html) -- initialize poor-quality reverb
  - [reverb](reverb.html) -- poor-quality reverb

## Other

  - [Offt](Offt.html) -- FFT analysis object

## Disk-based (non-realtime) Functions

There are a number of older 'disk-only' sound synthesis and
signal-processing functions that may be encountered in instrument
design. These were originally developed for the non-realtime *cmix*
music programming language from which RTcmix was derived. RTcmix
actually encapsulates all of the earlier *cmix* code, so that these
funcions that have not been ported to RTcmix still work. We include
documentation for these older disk-based functions mainly for those
compelling "historical" reasons, because none of them will access the
real-time audio stream of sound.

  - [ADDOUT](ADDOUT.html) -- add samples to disk
  - [GETIN](GETIN.html) -- get samples from disk
  - [LAYOUT](ADDOUT.html) -- write samples selectively to disk
  - [WIPEOUT](ADDOUT.html) -- write samples destructively to disk
  - [baddout](bgetin.html) -- add a block of samples to disk
  - [bgetin](bgetin.html) -- get a block of samples from disk
  - [blayout](bgetin.html) -- write a block of samples selectively to
    disk
  - [bwipeout](bgetin.html) -- write a block of samples destructively to
    disk
  - [endnote](endnote.html) -- update file header statistics at end of
    note
  - [getsample](getsample.html) -- fetch arbitrary block of samples from
    soundfile
  - [getsetnote](getsample.html) -- set up for getsample()
  - [inrepos](inrepos.html) -- reposition the input file pointer
  - [outrepos](inrepos.html) -- reposition the output file pointer
  - [setnote](setnote.html) -- set up soundfile for reading/writing

<span id="alphabetical" style="display:none;"> </span>

  - [Obucket](Obucket.html)
  - [Ocomb](Ocomb.html)
  - [Ocombi](Ocomb.html)
  - [Odcblock](Odcblock.html)
  - [Odelay](Odelay.html)
  - [Odelayi](Odelay.html)
  - [Oequalizer](Oequalizer.html)
  - [Offt](Offt.html)
  - [Oonepole](Oonepole.html)
  - [OonepoleTrack](OonepoleTrack.html)
  - [Ooscil](Ooscil.html)
  - [Ooscili](Ooscil.html)
  - [Orand](Orand.html)
  - [Oreson](Oreson.html)
  - [Ortgetin](Ortgetin.html)
  - [allpass](allpass.html)
  - [allpole](allpole.html)
  - [ampdb](ampdb.html)
  - [ballpole](ballpole.html)
  - [bbuzz](buzz.html)
  - [boost](ampdb.html)
  - [boscili](boscili.html)
  - [bpluck](bpluck.html)
  - [breson](breson.html)
  - [bresonz](bresonz.html)
  - [brrand](brrand.html)
  - [buzz](buzz.html)
  - [comb](allpass.html)
  - [combine\_gens](combine_gens.html)
  - [combset](allpass.html)
  - [cpsoct](ampdb.html)
  - [cpspch](ampdb.html)
