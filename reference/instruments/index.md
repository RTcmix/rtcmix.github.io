---
title: Instruments Reference
layout: ref
---

# Instruments Reference

An RTcmix instrument is a scorefile or interface-object command.  There are two main types: **synthesis instruments**, which generate audio, and **processing instruments**, which accept input audio and output a modified version of it.  There are also a small set of **special-case instruments** which perform other actions (see below). When an RTcmix instrument command is called, **RTcmix** instantiates a unique copy of itself with the parameters for the specific event (starting time, duration, etc.) included. This instrument/note object is then scheduled for execution at the appropriate starting time during playback.

## <span id="sort_topic">Listed by Topic</span>  

## Synthesis Instruments
These generate audio and send it via an aux or output bus.

### Wavetable
- [HALFWAVE](HALFWAVE.html) &mdash; constructed wavetable (synthesis)
- [MULTIWAVE](MULTIWAVE.html) &mdash; additive synthesis
- [SYNC](SYNC.html) &mdash; hard sync oscillator synthesis instrument
- [VWAVE](VWAVE.html) &mdash; vector wavetable synthesis
- [WAVETABLE](WAVETABLE.html) &mdash; wavetable oscillator
- [WAVESHAPE](WAVESHAPE.html) &mdash; waveshaping synthesis
- [WAVY](WAVY.html) &mdash; two-oscillator modulating synthesis
- [WIGGLE](WIGGLE.html) &mdash; wavetable oscillator with frequency modulation and filter

### Granular
- [GRANSYNTH](GRANSYNTH.html) &mdash; granular synthesis
- [JGRAN](JGRAN.html) &mdash; granular synthesis
- [SGRANR](SGRANR.html) &mdash; stochastic granular synthesis

### Noise
- [BROWN](BROWN.html) &mdash; brown noise instrument
- [CRACKLE](CRACKLE.html) &mdash; chaotic noise generator
- [DUST](DUST.html) &mdash; random impulses
- [HENON](HENON.html) &mdash; Henon map noise generator
- [LATOOCARFIAN](LATOOCARFIAN.html) &mdash; chaotic noise generator
- [NOISE](NOISE.html) &mdash; make noise
- [PINK](PINK.html) &mdash; pink noise instrument

### Frequency modulation
- [FMINST](FMINST.html) &mdash; frequency modulator (synthesis)
- [MULTIFM](MULTIFM.html) &mdash; configurable multi-oscillator FM synthesis instrument

### Miscellaneous
- [AMINST](AMINST.html) &mdash; amplitude modulator (synthesis)
- [LPCPLAY](LPCPLAY.html) &mdash; Linear Predective Coding (LPC) resynthesis
- [SCULPT](SCULPT.html) &mdash; frequency/amplitude pair-based resynthesis

### Physical Modeling
- [CLAR](CLAR.html) &mdash; early clarinet physical model
- [MBANDEDWG](MBANDEDWG.html) &mdash; banded waveguide (bars/modal things, struck & bowed) physical model
- [MBLOWBOTL](MBLOWBOTL.html) &mdash; simple Helmholtz resonator physical model
- [MBLOWHOLE](MBLOWHOLE.html) &mdash; clarinet physical model with tonehole and register vent
- [MBOWED](MBOWED.html) &mdash; bowed string physical model
- [MBRASS](MBRASS.html) &mdash; brass instrument physical model
- [MCLAR](MCLAR.html) &mdash; another clarinet physical model
- [METAFLUTE](METAFLUTE.html) &mdash; early, extended flute physical model
  * [*SFLUTE*](METAFLUTE.html#SFLUTE) &mdash; basic flute model
  * [*VSFLUTE*](METAFLUTE.html#VSFLUTE) &mdash; basic flute model with vibrato
  * [*BSFLUTE*](METAFLUTE.html#BSFLUTE) &mdash; basic flute model with pitch-bend
  * [*LSFLUTE*](METAFLUTE.html#LSFLUTE) &mdash; basic flute model for legato slurs
- [MMESH2D](MMESH2D.html) &mdash; waveguide model of a 2D mesh
- [MMODALBAR](MMODALBAR.html) &mdash; physical model of struck bars
- [MSAXOFONY](MSAXOFONY.html) &mdash; saxophone physical model
- [MSHAKERS](MSHAKERS.html) &mdash; "shaken" instrument physical models
- [MSITAR](MSITAR.html) &mdash; sitar physical model
- [STRUM](STRUM.html) &mdash; extended Karplus-Strong ("plucked string") algorithm, with distortion and feedback
  * [*START*](STRUM.html#START) &mdash; basic model
  * [*BEND*](STRUM.html#BEND) &mdash; basic model with pitch bend
  * [*FRET*](STRUM.html#FRET) &mdash; basic model fretted from previous note
  * [*START1*](STRUM.html#START1) &mdash; feedback/distortion model
  * [*BEND1*](STRUM.html#BEND1) &mdash; feedback/distortion model with pitch bend
  * [*FRET1*](STRUM.html#FRET1) &mdash; feedback/distortion model fretted from previous note
  * [*VSTART1*](STRUM.html#VSTART1) &mdash; feedback/distortion model with vibrato
  * [*VFRET1*](STRUM.html#VFRET1) &mdash; feedback/distortion model fretted from previous note, with vibrato
- [STRUM2](STRUM2.html) &mdash; tuned Karplus-Strong ("plucked string") algorithm
- [STRUMFB](STRUMFB.html) &mdash; extended Karplus-Strong ("plucked string") algorithm, with distortion and feedback

## Processing Instruments
These recieve audio from either an input file or an aux bus and send audio out via an aux or output bus.

### Mixing & Panning
- [MIX](MIX.html) &mdash; simple soundfile mixing command
- [NPAN](NPAN.html) &mdash; multichannel panning
- [PAN](PAN.html) &mdash; stereo panning
- [QPAN](QPAN.html) &mdash; 4-channel panning
- [REVMIX](REVMIX.html) &mdash; reverse input soundfile
- [STEREO](STEREO.html) &mdash; stereo mixing

### Transposing & Pitch shifting
- [MOCKBEND](MOCKBEND.html) &mdash; real-time pitch-shifter with dynamic modification of pitch
- [SCRUB](SCRUB.html) &mdash; fowards/backwards pitch shifter
- [TRANS](TRANS.html) &mdash; pitch-shifter
- [TRANS3](TRANS3.html) &mdash; pitch-shifter (3rd-order interpolation)
- [TRANSBEND](TRANSBEND.html) &mdash; pitch-shifter with dynamic modification of pitch

### Amplitude modulation & Distortion
- [AM](AM.html) &mdash; amplitude modulator (signal-processor)
- [COMPLIMIT](COMPLIMIT.html) &mdash; audio compressor/limiter
- [DECIMATE](DECIMATE.html) &mdash; reduce bit-representation of input sound amplitude
- [DISTORT](DISTORT.html) &mdash; distortion (clip) signal-procesor
- [FOLLOWER](FOLLOWER.html) &mdash; simple envelope (amplitude) follower
- [FOLLOWGATE](FOLLOWGATE.html) &mdash; envelope (amplitude) follower controlling an amplitude gate
- [SHAPE](SHAPE.html) &mdash; waveshape an input sound

### Filters & Equalizers
- [BUTTER](BUTTER.html) &mdash; time-varying Butterworth filter (high- or low-pass)
- [DCBLOCK](DCBLOCK.html) &mdash; remove (most of) DC bias from input signal
- [ELL](ELL.html) &mdash; elliptical filter
- [EQ](EQ.html) &mdash; equalizer instrument (peak/notch, shelving and high/low pass types)
- [FIR](FIR.html) &mdash; finite impulse response filter
- [FILTERBANK](FILTERBANK.html) &mdash; multi-band reson instrument (with dynamic control)
- [FILTSWEEP](FILTSWEEP.html) &mdash; time-varying biquad filter (band-pass)
- [FOLLOWBUTTER](FOLLOWBUTTER.html) &mdash; envelope (amplitude) follower controlling a Butterworth filter
- [HOLO](HOLO.html) &mdash; stereo FIR filter to perform crosstalk cancellation
- [IIR](IIR.html) &mdash; infinite impulse response filter
  * [*setup*](IIR.html#setup) &mdash; set up the IIR filter
  * [*INPUTSIG*](IIR.html#INPUTSIG) &mdash; filter an input signal
  * [*IINOISE*](IIR.html#IINOISE) &mdash; generate and filter noise
  * [*BUZZ*](IIR.html#BUZZ) &mdash; generate and filter a buzz signal
  * [*PULSE*](IIR.html#PULSE) &mdash; generate and filter a pulse signal
- [JFIR](JFIR.html) &mdash; finite impulse response filter specified by frequency curve
- [LPCIN](LPCPLAY.html) &mdash; Linear Predective Coding (LPC) resynthesis using input sound through the LPC filters
- [MOOGVCF](MOOGVCF.html) &mdash; dynamic resonant low-pass filter
- [MULTEQ](MULTEQ.html) &mdash; equalizer instrument with dynamic filter sections

### Vocoders
- [PVOC](PVOC.html) &mdash; phase vocoder
- [VOCODE2](VOCODE2.html) &mdash; channel vocoder
- [VOCODE3](VOCODE3.html) &mdash; a more flexible channel vocoder
- [VOCODESYNTH](VOCODESYNTH.html) &mdash; channel vocoder with oscillator-bank carrier

### Delays & Comb Filters
- [COMBIT](COMBIT.html) &mdash; comb filter
- [DEL1](DEL1.html) &mdash; single stereo delay
- [DELAY](DELAY.html) &mdash; simple regenerating delay
- [FLANGE](FLANGE.html) &mdash; notch or comb "flange" filter
- [JDELAY](JDELAY.html) &mdash; regenerating delay + low-pass filter
- [MULTICOMB](MULTICOMB.html) &mdash; four comb filters simultaneously
- [PANECHO](PANECHO.html) &mdash; stereo "ping-pong" regenerating delays

### Room simulation & Spacial Placement
- [DMOVE](DMOVE.html) &mdash; high-quality room simulation program for moving sources with dynamic control (multiple inputs)
- [FREEVERB](FREEVERB.html) &mdash; good-sounding reverbator
- [GVERB](GVERB.html) &mdash; good-sounding reverberator with long reverb times
- [LOCALIZE](LOCALIZE.html) &mdash; delay/amplitude/filter-based localization instrument
- [MMOVE](MMOVE.html) &mdash; high-quality room simulation program for moving sources (multiple inputs)
- [MPLACE](MPLACE.html) &mdash; high-quality room simulation program for stationary sources (multiple inputs)
- [MOVE](MOVE.html) &mdash; high-quality room simulation program for moving sources
- [MROOM](MROOM.html) &mdash; room simulation program for moving sources
- [PLACE](PLACE.html) &mdash; high-quality room simulation program for stationary sources
- [REV](REV.html) &mdash; three different reverberation algorithms
- [REVERBIT](REVERBIT.html) &mdash; Schroeder reverb
- [ROOM](ROOM.html) &mdash; delay line room-simulation model
- [SROOM](SROOM.html) &mdash; room simulation for stationary sources

### FFT-based

- [CONVOLVE1](CONVOLVE1.html) &mdash; FFT convolution
- [SPECTACLE](SPECTACLE.html) &mdash; FFT-based delay
- [SPECTACLE2](SPECTACLE2.html) &mdash; FFT-based delay (more real-time control)
- [SPECTEQ](SPECTEQ.html) &mdash; FFT-based EQ
- [SPECTEQ2](SPECTEQ2.html) &mdash; FFT-based EQ (more real-time control)
- [TVSPECTACLE](TVSPECTACLE.html) &mdash; FFT-based delay with time-varying properties

### Miscellaneous

- [CHAIN](CHAIN.html) &mdash; connect a set of instruments together so they execute as one
- [GRANULATE](GRANULATE.html) &mdash; granularize an input soundfile table
- [JCHOR](JCHOR.html) &mdash; granulated, random-wait chorus (signal-processor)
- [SPLITTER](SPLITTER.html) &mdash; output routing
- [STGRANR](STGRANR.html) &mdash; sampling stochastic granular processing

## Special-Case Instruments
These perform special actions and neither generate nor process audio.

### MIDI
- [MIDI](MIDI.html) &mdash; real-time scheduled MIDI control of an external device
  * [NOTE](MIDI.html#NOTE) &mdash; send MIDI note on events
  * [CONTROLLER](MIDI.html#CONTROLLER) &mdash; send MIDI controller events
  * [PITCHBEND](MIDI.html#PITCHBEND) &mdash; send MIDI pitch bend events
  * [PROGRAM](MIDI.html#PROGRAM) &mdash; send MIDI program change events

### Miscellaneous
- [DUMP](DUMP.html) &mdash; print control ('handle') data
- [MAXBANG](MAXBANG.html) &mdash; utility to generate a bang message in [rtcmix~](http://rtcmix.org/rtcmix~/) or [iRTcmix](http://rtcmix.org/iRTcmix/)
- [MAXMESSAGE](MAXMESSAGE.html) &mdash; utility to send a list of values, used in [rtcmix~\>](http://rtcmix.org/rtcmix~/) or [iRTcmix](http://rtcmix.org/iRTcmix/)
- [PFSCHED](PFSCHED.html) &mdash; schedule (real-time) pfield events

## <span id="sort_alphabetical">All Instruments, Listed in Alphabetical Order</span>

- [AM](AM.html)
- [AMINST](AMINST.html)
- [BROWN](BROWN.html)
- [BUTTER](BUTTER.html)
- [CHAIN](CHAIN.html)
- [CLAR](CLAR.html)
- [COMBFILT](COMBFILT.html)
- [COMBIT](COMBIT.html)
- [COMPLIMIT](COMPLIMIT.html)
- [CONVOLVE1](CONVOLVE1.html)
- [CRACKLE](CRACKLE.html)
- [DCBLOCK](DCBLOCK.html)
- [DECIMATE](DECIMATE.html)
- [DEL1](DEL1.html)
- [DELAY](DELAY.html)
- [DISTORT](DISTORT.html)
- [DMOVE](DMOVE.html)
- [DUMP](DUMP.html)
- [DUST](DUST.html)
- [ELL](ELL.html)
- [EQ](EQ.html)
- [FEEDBACK](FEEDBACK.html)
- [FILTERBANK](FILTERBANK.html)
- [FILTSWEEP](FILTSWEEP.html)
- [FIR](FIR.html)
- [FLANGE](FLANGE.html)
- [FMINST](FMINST.html)
- [FOLLOWBUTTER](FOLLOWBUTTER.html)
- [FOLLOWER](FOLLOWER.html)
- [FOLLOWGATE](FOLLOWGATE.html)
- [FREEVERB](FREEVERB.html)
- [GRANSYNTH](GRANSYNTH.html)
- [GRANULATE](GRANULATE.html)
- [GVERB](GVERB.html)
- [HALFWAVE](HALFWAVE.html)
- [HAR](HAR.html)
- [HENON](HENON.html)
- [HOLO](HOLO.html)
- [IIR](IIR.html)
  * [setup](IIR.html#setup)
  * [INPUTSIG](IIR.html#INPUTSIG)
  * [IINOISE](IIR.html#IINOISE)
  * [BUZZ](IIR.html#BUZZ)
  * [PULSE](IIR.html#PULSE)
- [JCHOR](JCHOR.html)
- [JDELAY](JDELAY.html)
- [JFIR](JFIR.html)
- [JGRAN](JGRAN.html)
- [LATOOCARFIAN](LATOOCARFIAN.html)
- [LOCALIZE](LOCALIZE.html)
- [LOOP](LOOP.html)
- [LPCPLAY](LPCPLAY.html)
- [LPCIN](LPCPLAY.html)
- [MAXBANG](MAXBANG.html)
- [MAXMESSAGE](MAXMESSAGE.html)
- [MBANDEDWG](MBANDEDWG.html)
- [MBLOWBOTL](MBLOWBOTL.html)
- [MBLOWHOLE](MBLOWHOLE.html)
- [MBOWED](MBOWED.html)
- [MBRASS](MBRASS.html)
- [MCLAR](MCLAR.html)
- [METAFLUTE](METAFLUTE.html)
  * [SFLUTE](METAFLUTE.html#SFLUTE)
  * [VSFLUTE](METAFLUTE.html#VSFLUTE)
  * [BSFLUTE](METAFLUTE.html#BSFLUTE)
  * [LSFLUTE](METAFLUTE.html#LSFLUTE)
- [MIDI](MIDI.html)
  * [NOTE](MIDI.html#NOTE)
  * [CONTROLLER](MIDI.html#CONTROLLER)
  * [PITCHBEND](MIDI.html#PITCHBEND)
  * [PROGRAM](MIDI.html#PROGRAM)
- [MIX](MIX.html)
- [MIXN](MIXN.html)
- [MMESH2D](MMESH2D.html)
- [MMODALBAR](MMODALBAR.html)
- [MMOVE](MMOVE.html)
- [MPLACE](MPLACE.html)
- [MOCKBEND](MOCKBEND.html)
- [MOOGVCF](MOOGVCF.html)
- [MOVE](MOVE.html)
- [MROOM](MROOM.html)
- [MSAXOFONY](MSAXOFONY.html)
- [MSHAKERS](MSHAKERS.html)
- [MSITAR](MSITAR.html)
- [MULTEQ](MULTEQ.html)
- [MULTICOMB](MULTICOMB.html)
- [MULTIFM](MULTIFM.html)
- [MULTIWAVE](MULTIWAVE.html)
- [NOISE](NOISE.html)
- [NPAN](NPAN.html)
- [PAN](PAN.html)
- [PANECHO](PANECHO.html)
- [PHASER](PHASER.html)
- [PFSCHED](PFSCHED.html)
- [PINK](PINK.html)
- [PLACE](PLACE.html)
- [PVOC](PVOC.html)
- [QPAN](QPAN.html)
- [RAP](RAP.html)
- [REV](REV.html)
- [REVERBIT](REVERBIT.html)
- [REVMIX](REVMIX.html)
- [ROOM](ROOM.html)
- [SCRUB](SCRUB.html)
- [SCULPT](SCULPT.html)
- [SGRANR](SGRANR.html)
- [SHAPE](SHAPE.html)
- [SPECTACLE](SPECTACLE.html)
- [SPECTACLE2](SPECTACLE2.html)
- [SPECTEQ](SPECTEQ.htm)
- [SPECTEQ2](SPECTEQ2.html)
- [SPLITTER](SPLITTER.html)
- [SROOM](SROOM.html)
- [STEREO](STEREO.html)
- [STGRANR](STGRANR.html)
- [STRUM](STRUM.html)
	* [START](STRUM.html#START)
	* [BEND](STRUM.html#BEND)
	* [FRET](STRUM.html#FRET)
	* [START1](STRUM.html#START1)
	* [BEND1](STRUM.html#BEND1)
	* [FRET1](STRUM.html#FRET1)
	* [VSTART1](STRUM.html#VSTART1)
	* [VFRET1](STRUM.html#VFRET1)
- [STRUM2](STRUM2.html)
- [STRUMFB](STRUMFB.html)
- [SYNC](SYNC.html)
- [TRANS](TRANS.html)
- [TRANS3](TRANS3.html)
- [TRANSBEND](TRANSBEND.html)
- [TVSPECTACLE](TVSPECTACLE.html)
- [VOCODE2](VOCODE2.html)
- [VOCODE3](VOCODE3.html)
- [VOCODESYNTH](VOCODESYNTH.html)
- [VWAVE](VWAVE.html)
- [WAVETABLE](WAVETABLE.html)
- [WAVESHAPE](WAVESHAPE.html)
- [WAVY](WAVY.html)
- [WIGGLE](WIGGLE.html)
