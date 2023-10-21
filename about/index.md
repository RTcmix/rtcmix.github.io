---
title: The Ancient History of RTcmix
layout: ref
---

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
list](https://listserv.cuit.columbia.edu/scripts/wa.exe?SUBED1=rtcmix-discuss&A=1), etc.

We hope you find the language useful\!

Brad Garton

