---
title: About RTcmix
layout: ref
---

# About RTcmix

RTcmix compiles and runs on most Unix-like systems, including various
flavors of Linux, macOS, IRIX, FreeBSD. A windows port is available
making use of the max/msp [rtcmix\~](../rtcmix_/index.html) object.

RTcmix currently includes the following components:

  - a library of low-level C/C++ functions and objects for performing
    most contemporary digital audio and signal-processing tasks
  - a substantial set of pre-coded "instruments" instantiating a variety
    of DSP and sound-synthesis algorithms (click
    [here](../reference/instruments/index.html) for a list)
  - a fully-featured command-parsing langauge to allow easy
    incorporation of algorthmic control procedures in sound generation
  - an option to allow the perl or python programming languages to be
    used as the control/command-parsing environment for RTcmix
  - a robust and sample-accurate scheduler for timing and arbitrary
    event-scheduling
  - an 'embedded' RTcmix object and associated library to enable the
    entire RTcmix language (scheduler too\!) to be compiled and used
    seamlessly within other C/C++ applications
  - a TCP/IP socket interface for external control of RTcmix from other
    processes or machines
  - the physical model and PhISEM routines from Perry Cook and Gary
    Scavone's [Synthesis ToolKit
    (STK)](http://www-ccrma.stanford.edu/software/stk/) as well as
    affiliated RTcmix instruments using the stk routines
  - the ability to read/write most contemporary soundfile formats by via
    Bill Schottstaedt's
    [sndlib](http://www-ccrma.stanford.edu/software/snd/snd/sndlib.html)
  - a package of examples showing RTcmix use with MIDI, X11/motif,
    wxWindows, Lisp, Open Sound Control (OSC), OpenGL, etc.
  - a set of command-line utility programs for playing and manipulating
    soundfiles
  - a Max/MSP [rtcmix\~](../rtcmix_/index.html) object, available for
    both macOS and Windows versions of max/msp  
  - dynamic p-field modification (in version 4.0)  
  - "pull"-model for audio i/o (JACK, PortAudio, etc.)

RTcmix is derived from the original CMIX language, developed at
Princeton University by [Paul
Lansky](http://paul.mycpanel.princeton.edu/).

## Contributors

[Brad Garton](http://bradgarton.com/)

[Dave Topper](http://www.davetopper.com/)

[John Gibson](http://john-gibson.com/)

[Doug Scott](http://music.columbia.edu/~doug)

[Luke DuBois](http://www.lukedubois.com/)

[Mara Helmuth](http://www.marahelmuth.com/)

[Chris Bailey](http://music.columbia.edu/~chris)

[Stanko Juzbasic](http://music.columbia.edu/~stanko)

[Ico Bukvic](http://ico.bukvic.net/)

[Joel Matthys](http://joel.matthysmusic.com/)

[Damon Holzborn](http://damonholzborn.com/)

## The Ancient History of RTcmix

Luke Dubois wrote a [brief history of
RTcmix](http://music.columbia.edu/cmix/history.html) back in the
mid-1990's, quoting from an earlier brief history of CMIX written by
Brad Garton (me\!). The histories are in fact still historical, but a
few recent (c. 2000-2003) RTcmix events worth noting -- if you are the
kind that likes noting these things.

With the demise of SGI machines as a semi-platform-of-choice for the
computer music community, RTcmix went into a period of 'underground'
usage. As noted in Luke's document, it was ported to Linux and was
further developed by a core group of RTcmix users who also adopted Linux
for musical work. Dave Topper and John Gibson at the University of
Virginia (John is presently at Indiana University) and Doug Scott
(formerly of SGI, now with Apple) in particular added extensive new
features to the language and greatly expanded RTcmix capabilities --
perl/python interface, instrument interconnecting ability, much larger
instrument base, etc.

But by and large, not too many new users began working with RTcmix,
primarily because we never took the time to create a coherent body of
documentation. Luke, followed with additional work by Dave and John,
added to the tiny existing RTcmix documentation (and much of their work
has been incorporated into these web pages), but to use RTcmix you
sort-of had to know how to use it already.

I was still using RTcmix a fair amount in my own musical work, but I
also began to look at other languages (like
[JSyn](http://www.softsynth.com/jsyn/),
[Max/MSP](http://www.cycling74.com/products/maxmsp.html) and
[SuperCollider](http://www.audiosynth.com/)). One of the things I didn't
like about these languages was the difficulty in incorporating them into
C/C++ applications with a high degree of data-sharing. I also
rediscovered how much I enjoy the straightforward algorthmic processing
capabilities of RTcmix. So, with help from Doug/Dave/John, I wrote an
'embedded' RTcmix object to facilitate the C/C++ connection, and decided
to get our documentation house in order so that others who might want a
language like RTcmix could gain entry.

I had noticed that many of our students at Columbia were adopting
[Max/MSP](http://www.cycling74.com/products/maxmsp.html) as a base for
their computer music operations. The Max/MSP
[rtcmix\~](../rtcmix_/index.html) object was created to bring the Joy of
RTcmix to this environment. The recent Windows XP port of this object
allowed us to build an executable RTcmix for Windows without too much
additional pain.

That's the story so far. With a solid Linux version and an equally solid
port to macOS (and the hybrid RTcmix-Max/MSP Windows version), we
believe that RTcmix should be attractive for other
programmers-musicians-audio people. Please browse through the
documentation here (especially the
[tutorials](../tutorials/index.html)), [download](../rtcmix/index.html)
and try a few instruments, join the [RTcmix discussion
list](https://lists.columbia.edu/mailman/listinfo/rtcmix-discuss), etc.

We hope you find the language useful\!

Brad Garton

