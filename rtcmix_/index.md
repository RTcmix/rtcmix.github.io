---
title: rtcmix~
layout: ref
---

# rtcmix~

RTcmix is a complete sound synthesis and signal processing language,
including a robust scheduler and large set of pre-compiled
"instruments". The rtcmix~ object completely encapsulates RTcmix within
the [Max/MSP](http://cycling74.com/products/max/) and [Pure
Data](http://puredata.info/) real-time music environments.

## Installation Instructions for Max 7 and 8

### macOS

1.  [Download
    rtcmix~](http://rtcmix.org/rtcmix~/downloads/RTcmix-2.01.zip).
2.  Unpack the archive.
3.  Move the entire "RTcmix-2.01" folder intact to the directory
    "/Users/Sharer/Max 7/Library/" or "/Users/Sharer/Max 8/Library/"
    folder.
4.  Start (or restart) Max/MSP. \[rtcmix~\] should be working.

<span class="small">***Note:** If you are installing \[rtcmix~\] on
macOS machines running Catalina (10.15.x) or later, you will probably
get the Apple 'gatekeeper' message about the library/application not
able to run because it isn't from a 'signed developer'. One of these
days We'll wade through the impenetrable documentation about how to do
this, but for now you can follow the instructions
[here](http://rtcmix.org/rtcmix~/annoying.php) to get it going.*</span>

### Windows 10

1.  [Download
    rtcmix~](http://rtcmix.org/rtcmix~/downloads/rtcmix~-for-windows-maxmsp.zip).
2.  Unpack the archive.
3.  Move the entire "rtcmix~-for-windows-maxmsp" folder intact to your
    "Documents\Max 7\Library\\ or your "Documents\Max 8\Library\\
    folder.
4.  Move the "pthreadVC2.dll" file to the main "C:\Program Files\Cycling
    '74\Max 7\\ or "C:\Program Files\Cycling '74\Max 8\\ folder.
5.  Start (or restart) Max/MSP. \[rtcmix~\] should be working.

<span class="small">***Note on editing scorefiles on Windows:** You can
use an external editor to create \[rtcmix~\] scorefiles. However, be
sure that the editor can write "unix" or "OSX" line endings, *not*
Windows line endings. Windows uses a combination
carriage-return/line-feed scheme that will cause the RTcmix parser to
crash.*</span>

Older versions are available [in the
archives](archives.html).

## Installation Instructions for Pure Data

### macOS

1.  [Download
    rtcmix~](http://rtcmix.org/rtcmix~/downloads/rtcmix~-for-pd.zip).
2.  Unpack the archive.
3.  Inside the "rtcmix~-for-pd" folder is an "rtcmix~" folder. Copy/move
    that entire folder intact to your "Documents/pd/externals" folder.
4.  Start (or restart) Pd. \[rtcmix~\] should be working.

<span class="small">***Note:** If you are installing \[rtcmix~\] on
macOS machines running Catalina (10.15.x) or later, you will probably
get the Apple 'gatekeeper' message about the library/application not
able to run because it isn't from a 'signed developer'. One of these
days We'll wade through the impenetrable documentation about how to do
this, but for now you can follow the instructions
[here](http://rtcmix.org/rtcmix~/annoying.php) to get it going.*</span>

<span class="small">***Note on editing scorefiles on macOS:** At
present, scorefile information is not stored with the pd patch when it
is saved. You will need to use the \[open\] message to load an existing
scorefile into \[rtcmix~\], and be sure to use a \[save\]/\[saveas\]
message to store a scorefile that has been modified within pd.*</span>

<span class="small"></span>

To modify a scorefile within pd, click on the \[rtcmix~\] object in a
locked (i.e. editing off) pd patcher. This will open an editor on the
current \[rtcmix~\] script. If the script has not been loaded with an
\[open\] message, \[rtcmix~\] will create a temporary file for editing.
BE CERTAIN TO SAVE THIS using the \[save\]/\[saveas\] message before
exiting pd, or the work will be lost.

<span class="small">*In this version, the ability to select a particular
editor using the \[editor\] message does not function on OSX.*</span>

### Windows 10

1.  [Download
    rtcmix~](http://rtcmix.org/rtcmix~/downloads/rtcmix~-for-windows-maxmsp.zip).
2.  Unpack the archive.
3.  Move the entire "rtcmix~" folder intact to your
    "Documents\Pd\externals\\ folder.
4.  Move the "pthreadVC2.dll" file to the main "C:\Program
    Files\Pd\bin\\ folder.
5.  Start (or restart) Max/MSP. \[rtcmix~\] should be working.

<span class="small">***Note on editing scorefiles on Windows:** At
present, scorefile information is not stored with the pd patch when it
is saved. You will need to use the \[open\] message to load an existing
scorefile into \[rtcmix~\], and be sure to use a \[save\]/\[saveas\]
message to store a scorefile that has been modified within pd.*</span>

<span class="small"></span>

To modify a scorefile within pd, click on the \[rtcmix~\] object in a
locked (i.e. editing off) pd patcher. This will open an editor on the
current \[rtcmix~\] script. If the script has not been loaded with an
\[open\] message, \[rtcmix~\] will create a temporary file for editing.
BE CERTAIN TO SAVE THIS using the \[save\]/\[saveas\] message before
exiting pd, or the work will be lost.

The \[rtcmix~\] here comes configured to use the popular "Notepad++"
editor (https://notepad-plus-plus.org/). The configuration assumes that
the editor is stored in the default location "C:\Program
Files\Notepad++\notepad++.exe".

You can use an alternative editor and set \[rtcmix~\] to invoke it using
the \[editor\] message. The \[editor\] message takes one argument: the
pathname to the editor program. This pathname has to be specified using
unix-like directory separators. For example, an editor located at
"C:\Program Files\thebest\editor.exe" would be set by sending the
message:

       [editor C:/Program\ Files/thebest/editor.exe]

to an \[rtcmix~\] object (note the escape (\\ ) of the space in the
pathname, too). Sending this message to a single \[rtcmix~\] object will
set it as the default for all \[rtcmix~\] objects in a given pd session.
The message will need to be resent if pd is restarted.

<span class="small">*You can also use an external editor to create
\[rtcmix~\] scorefiles. However, be sure that any alternative editors
you use can write "unix" or "OSX" line endings, not Windows line endings
(Notepad++ can do this, but the plain Notepad that comes with Windows
cannot). Native Windows text files use a combination
carriage-return/line-feed scheme that will cause the RTcmix parser to
crash.*</span>

## Features

- The rtcmix~ object can load, parse and run existing *RTcmix*
  scorefiles. A set of internal buffers and buffer-editing routines are
  included with the object.

- A large set of mathematical and data-manipulation/storage routines are
  available with the rtcmix~ object, including the ability to define and
  use arbitrary new operations.

- rtcmix~ enables the Max/MSP user to write procedural code for
  particular algorithmic operations. For example, a valid *RTcmix*
  script executing in the rtcmix~ object might be something like:

- Approximately 120 existing *RTcmix* synthesis and signal-processing
  instruments are currently accessible in the rtcmix~ object, including
  a set of FFT/PVOC-based spectral manipulation tools, real-time Linear
  Prediction Coding (LPC) analysis/resynthesis, and most of the
  [Synthesis ToolKit (STK)](http://www-ccrma.stanford.edu/software/stk/)
  physical models created by Perry Cook and Gary Scavone.

- In a similar fashion, the rtcmix~ object can schedule Max/MSP messages
  and events. The following rtcmix~ script will produce 100 'bangs'
  randomly spaced in a 7-second interval:

         for (i = 0; i < 100; i = i+1) {
              bangtime = irand(0.0, 7.0)
              MAXBANG(bangtime)
          }

- rtcmix~ provides an easy framework for linkage between Max/MSP and
  arbitrary C/C++ functions and objects, including separately-compiled
  mach-o C/C++ code.

## Final Words on This Web Page

RTcmix was originally written by Brad Garton and Dave Topper, adding
real-time capabilities to the cmix music-programming language developed
by Paul Lansky. John Gibson, Doug Scott (and others) added significant
extensions to the package.

The rtcmix~ object was written by Brad Garton with much advice and
assistance from Dan Trueman and Luke DuBois (Dan wrote the internal
buffer script-editing code). Joshua Kit Clayton was an invaluable
resource, as always. Thanks guys!

I hope this may be useful for others; I'm having a blast with it. Let me
know what you think!

Brad Garton

  
  
