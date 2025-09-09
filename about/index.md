---
title: The History of cmix and RTcmix
layout: ref
---

# <a name=history></a>The "Ancient" History of RTcmix

Luke Dubois wrote a [brief history of RTcmix](http://sites.music.columbia.edu/cmc/cmix_dir/cmix_docs/history.html)
back in the mid-1990's, quoting from an earlier brief history of CMIX
written by Brad Garton (me). The histories are in fact still historical, but a
few recent (c. 2000-2003) RTcmix events worth noting -- if you are the
kind that likes noting these things.

With the demise of SGI machines as a semi-platform-of-choice for the
computer music community, RTcmix went into a period of 'underground'
usage. As noted in Luke's document, it was ported to Linux and was
further developed by a core group of RTcmix users who also adopted Linux
for musical work. Dave Topper and John Gibson at the University of
Virginia (John is presently at Indiana University) and Doug Scott
(formerly of Apple, now retired) in particular added extensive new
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
[JSyn](https://www.softsynth.com/jsyn/),
[Max/MSP](https://www.cycling74.com/products/max) and
[SuperCollider](https://www.audiosynth.com/)). One of the things I didn't
like about these languages was the difficulty in incorporating them into
C/C++ applications with a high degree of data-sharing. I also
rediscovered how much I enjoy the straightforward algorthmic processing
capabilities of RTcmix. So, with help from Doug/Dave/John, I wrote an
'embedded' RTcmix object to facilitate the C/C++ connection, and decided
to get our documentation house in order so that others who might want a
language like RTcmix could gain entry.

I had noticed that many of our students at Columbia were adopting
[Max/MSP](https://www.cycling74.com/products/max) as a base for
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
list](https://listserv.cuit.columbia.edu/scripts/wa.exe?SUBED1=rtcmix-discuss&A=1), etc.

We hope you find the language useful\!

Brad Garton

# <a name=timeline></a>Timeline of RTcmix Code Development

- \<1985
	* Paul Lansky develops a **C**-language version of the Fortran-based **MIX**.
- 1985
	* Date on oldest known pre-real-time cmix distribution source file.
- 1995
	* First work done on what would become **RTcmix** on the Silicon Graphics IRIX platform.
- 1998
	* Last development work done on pre-real-time cmix.
- 1999
	* First version of **RTcmix** source made available.
- 2000
	* Version 2.2.3 introduces new bus architecture.
	* [Perl](../tutorials/perl.html) support introduced.
	* Versions 3.0.0 through 3.0.4.
- 2001
	* Version 3.0.5.
	* [Python](../tutorials/python.html) support introduced.
	* Initial support for MacOS X.
- 2002
	* First attempt at real-time pfield control.
	* Versions 3.1 through 3.3.
- 2003
	* Version 3.4.
- 2004
	* Full support for dynamic, real-time [pfields](../tutorials/pfields.html)
	* Version 3.6 (3.5 skipped).
- 2005
	* Version 4.0 released!
- 2006
	* JACK support added.
- 2007 - 2008
	* New instruments, bug fixing
- 2009
	* 64-bit MacOS version released.
- 2010
- 2011
	* Multi-threaded MacOS X version released.
- 2012 - 2013
	* New instruments, bug fixing
- 2014
	* Version 4.1
	* **RTcmix** source uploaded to [Github](https://github.com/RTcmix)
- 2015
	* **Minc** <a href="../reference/scorefile/Minc.html#minc-functions">user-defined function</a> support plus scoped variables.
	* Version 4.2
- 2016
	* <a href="../reference/scorefile/Minc.html#command-line-named-args">Named-argument</a> variable support.
- 2017
	* Parser error handling improved.
- 2018
- 2019
	* Version 4.3.2 released.
	* Support for <a href="../reference/scorefile/Minc.html#struct">struct</a> data type.
	* Version 4.4
- 2020
	* Support for other new **Minc** data types.
	* Version 4.5
	* **OSC** support added.
	* Version 4.6
	* Version 5.0 released!
	* Version 5.1
- 2021
- 2022
	* Structs now have "class" behavior with method functions.
	* Version 5.2
- 2023
	* Version 5.2.1 through 5.3
- 2024
	* Version 5.4 and 5.5
- 2025
	* Version 5.6 and 5.7

