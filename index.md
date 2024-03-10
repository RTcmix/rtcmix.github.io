---
title: Introduction to RTcmix
layout: ref
---

# Introduction to RTcmix

## What is RTcmix?
**RTcmix** is a real-time software system for synthesizing and processing digital audio. It has two main parts: a front-end parsing system which reads ASCII scripts or “scores“ written using the [Minc language](reference/scorefile/Minc.html), and a powerful back-end audio rendering engine which performs the event scheduling, audio I/O, sound synthesis and processing. It is written in C/C++ and is distributed open-source, free of charge. In certain respects, it is similar in function to other extant unit-generator-based software packages such as [CSOUND](https://www.csounds.com/),
[SuperCollider](https://supercollider.sourceforge.net/) and (to a lesser
extent) [JSyn](https://www.softsynth.com/jsyn/) and
[Max/MSP](https://www.cycling74.com/products/max) -- they do
share a common heritage, after all. There are differences, however,
between all these packages... and *variety is* of course *the spice of
life\!*

RTcmix compiles and runs on most Unix-like systems, including various
flavors of Linux, macOS, IRIX, FreeBSD. A Windows port is available
making use of the Max/MSP [rtcmix\~](../rtcmix_/index.html) object.

So if you've been searching the web high and low for just the right
library of DSP functions to include in your latest\&greatest "killer"
(or maybe "peacefully coexisting"?) app, then **RTcmix** may just be the
Right Package for You.

## How Can I Try it Out?
If you are working on a Mac or a Windows machine, the easiest way to experiment with RTcmix is to download, install, and run John Gibson’s [RTcmixShell application](https://cecm.indiana.edu/rtcmix/rtcmix-app.html).  To make a start, paste the following three lines into the app's score window and press “play”:

```cpp
WAVETABLE(0, 4, 5000, 440)
WAVETABLE(1, 3, 5000, 440*3/2)
WAVETABLE(2, 2, 5000, 440*5/4)
```
There.  You just made your first score, using the [WAVETABLE](reference/instruments/WAVETABLE.html) instrument command.  You can experiment with most of what is described in the [reference pages](reference/index.html) using this application.

## How Else Can I Use It?
**RTcmix** can be configured and compiled to run in several different modes:

  - **[Standalone](standalone/index.html)**  
    A command line version for most Unix-like systems, including various flavors of Linux, MacOS X, IRIX, and FreeBSD.
  - **[rtcmix\~](rtcmix_/index.html)**  
    All functionality compiled into an object for use in the Max/MSP and Pd graphical programming environments.
  - **[Embedded](embedded/index.html)**  
    All functionality, accessible via a public API, compiled into a dynamic library which can be linked into a larger application (which then handles audio I/O and rendering callbacks). [RTcmixShell](https://cecm.indiana.edu/rtcmix/rtcmix-app.html) is an example of such an app.
<!--  - **[iRTcmix](irtcmix/index.html)**   --> 
<!--     For use in iPhone and iPad apps. --> 

## Where Do I Download It?
If you are happy with the capabilities of [RTcmixShell](https://cecm.indiana.edu/rtcmix/rtcmix-app.html), you don't need to download anything else!  The app already contains the embedded library described above.  For the others, depending on which configuration (mode) you decide to use, **RTcmix** is downloadable as either a precompiled binary or as configurable, compilable source code.
  
- If you want to use **RTcmix** as a standalone Unix-style command, follow the step-by-step instructions for downloading, configuring and building the source on various platforms [here](standalone/index.html).
- If you want to use **RTcmix** as a Max/MSP object, follow the instructions for **rtcmix\~** [here](rtcmix_/index.html).
- If you want to build an **RTcmix** dynamic library to embed in a larger software project, follow the step-by-step instructions for downloading, configuring and building the source on various platforms [here](embedded/index.html) *but note that documentation for doing this on your own is a work-in-progress*.

## More About RTcmix

RTcmix currently includes the following components:

  - a library of low-level C/C++ functions and objects for performing
    most contemporary digital audio and signal-processing tasks
  - a substantial set of precompiled "instruments" instantiating a variety
    of DSP and sound-synthesis algorithms (click
    [here](../reference/instruments/index.html) for a list)
  - a fully-featured [command-parsing language](reference/scorefile/Minc.html) to allow easy
    incorporation of algorthmic control procedures in sound generation
  - an option to allow the Perl or Python programming languages to be
    used as the control/command-parsing environment for RTcmix
  - a robust and sample-accurate scheduler for timing and arbitrary
    event-scheduling
  - an 'embedded' RTcmix object and associated library to enable the
    entire RTcmix system (scheduler too\!) to be compiled and used
    seamlessly within other C/C++ applications
  - a TCP/IP socket interface for external control of RTcmix from other
    processes or machines
  - the physical model and PhISEM routines from Perry Cook and Gary
    Scavone's [Synthesis ToolKit
    (STK)](https://ccrma.stanford.edu/software/stk/) as well as
    affiliated RTcmix instruments using the stk routines
  - the ability to read/write most contemporary soundfile formats by via
    Bill Schottstaedt's
    [sndlib](https://ccrma.stanford.edu/software/snd/snd/sndlib.html)
  - a package of examples showing RTcmix use with MIDI, X11/Motif,
    wxWindows, Lisp, Open Sound Control (OSC), OpenGL, etc.
  - a set of command-line utility programs for playing and manipulating
    soundfiles
  - a Max/MSP [rtcmix\~](../rtcmix_/index.html) object, available for
    both macOS and Windows versions of max/msp  
  - dynamic parameter modification  
  - "pull" model for audio I/O (JACK, PortAudio, etc.)

RTcmix is derived from the original CMIX software, developed at
Princeton University by [Paul
Lansky](https://paul.mycpanel.princeton.edu/).

## Contributors

[Brad Garton](https://bradgarton.com/)

[Dave Topper](https://www.davetopper.com/)

[John Gibson](http://john-gibson.com/)

[Doug Scott](https://music.columbia.edu/~doug)

[Mara Helmuth](https://www.marahelmuth.com/)

[Luke DuBois](https://www.lukedubois.com/)

[Chris Bailey](https://music.columbia.edu/~chris)

[Stanko Juzbasic](https://music.columbia.edu/~stanko)

[Ico Bukvic](https://ico.bukvic.net/)

[Joel Matthys](https://joel.matthysmusic.com/)

[Damon Holzborn](https://damonholzborn.com/)

---

<a href="https://github.com/rtcmix">github.com/rtcmix</a>
