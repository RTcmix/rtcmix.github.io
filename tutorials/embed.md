---
title: Using RTcmix Embedded Inside Another Application
layout: ref
---

# Using RTcmix Embedded Inside Another Application

One of the niftiest things about RTcmix is that almost the entire
language can be "embedded" within another C++ application. Direct
interfaces to RTcmix instruments can be designed quite easily using
graphical environments such as wxWindows, X11/Motif, OpenGL, etc. or
RTcmix can be used as a convenient way to "auralize" data within an
entirely different program, or certain features of RTcmix (like the
scheduler) can be imported for totally twisted non-audio use, or...
like, the sky's the limit. Yeah. That's it. Imagination.

It is really simple to set this up. To access and run RTcmix from within
another program, you need to make use of the [RTcmix
object](../reference/interface/RTcmix-embed.html). To use this, you will
need the statements

``` 
#define MAIN
#include <globals.h>
#include <RTcmix.h>
```

in the file containing the *main()* entry point. The "globals.h" file
contains definitions of variables and values that RTcmix needs. The
"RTcmix.h" file is included for the defintion of the RTcmix object as
you would with any C++ object (we are assuming that the Makefile is set
appropriately to find the RTcmix.h file -- see the discussion of
Makefiles below). The "\#define MAIN" is needed by RTcmix to be sure
that certain variable definitions are appropriately included.

You will also need to put

``` 
#include <globals.h>
#include <RTcmix.h>
```

in any subsidiary files defining functions or objects that make use of
the RTcmix object, as you would expect (note that the "\#define MAIN"
statement only goes in the same file where *main()* is defined).

With these \#include files set, creating the RTcmix object is trivial:

``` 
RTcmix *rrr;

rrr = new RTcmix();
```

At this point, *rrr* references a fully-functioning RTcmix. The

``` 
rrr = new RTcmix();
```

statement takes the place of the *rtsetparams* scorefile command. The
*RTcmix()* constructor can set many of the same parameters as
*rtsetparams* -- sampling rate, number of channels, etc. -- if so
desired.

All scorefile commands can now be sent to the RTcmix object using this
syntax:

``` 
rrr->cmd("COMMAND",   NPARAMS,   p0, p1, p2, ...);
```

where *COMMAND* is the name of the scorefile command, *NPARAMS* is the
number of parameters you are sending the command, and the following *p0,
p1, p2, ...* numbers are the parameter values. The only trickiness in
this vs. an actual scorefile command is that the p-fields should be
floating-point values (for numerical parameters) or "string" values (for
strings, obviously). *NPARAMS* should be an integer. Regular RTcmix
scorefiles allow both floating-point and integere p-fields, but the
truth of the matter is that they are all converted to floating-point
'inside' the language, so this really doesn't change how a command will
function.

For example, the WAVETABLE command we used in our first [simple
standalone tutorial](standalone.html)

``` 
WAVETABLE(0, 3.5, 20000, 440.0)
```

would be sent to RTcmix via the *rrr* object we created like this:

``` 
rrr->cmd("WAVETABLE", 4, 0.0, 3.5, 20000.0, 440.0);
```

The entire "greatmusic.score" scorefile:

``` 
rtsetparams(44100, 2)
load("WAVETABLE")

makegen(1, 24, 1000, 0,1, 3.5,1)
makegen(2, 10, 1000, 1.0, 0.4, 0.2)

WAVETABLE(0, 3.5, 20000, 440.0)
```

in an embedded application would be:

``` 
RTcmix *rrr;

rrr = new RTcmix(44100.0, 2); // not completely necessary -- this is the default
sleep(1);

rrr->cmd("load", 1, "WAVETABLE");

rrr->cmd("makegen", 7, 1.0, 24.0, 1000.0, 0.0, 1.0, 3.5, 1.0);
rrr->cmd("makegen", 6, 2.0, 10.0, 1000.0, 1.0, 0.4, 0.2);

rrr->cmd("WAVETABLE", 4, 0.0, 3.5, 20000.0, 440.0);
```

The *sleep(1)* is sometimes needed to allow the computer to fully
instantiate the RTcmix thread. On faster machines this isn't necessary.

The only part of RTcmix that the RTcmix object does *not* handle is the
Minc (or perl, python, etc.) "front-end" parsing language. Thus no *for*
loops, or *if-then-else* constructions can be sent to the RTcmix object.
We assume that the embedded context will allow you to do any of this --
the application in which the RTcmix object functions <u>becomes</u> the
interface/parser. It would be somewhat silly to build and send a loop
construct from within C++ when you could just do it in C++, right?

## The RTcmix Object Return Value

When the RTcmix object completes the execution of a command, what does
it return? If the command was an RTcmix instrument, it returns an
*Instrument \** pointer that can be recast to a specific type of
instrument. Otherwise the value it returns is rather meaningless and
must be retrieved in a different way (see below).

The reason that this *Instrument \** pointer is important is that it
gives the embedding application a way to interact directly with each
RTcmix note that gets scheduled. Suppose you wanted to change the
frequency of an executing WAVETABLE note dynamically, perhaps tracking
the movement of an interface slider or some other changing data object.
By designing and adding a method for WAVETABLE like:

``` 
WAVETABLE::changeFrequency(double freq)
```

you can use the returned *Instrument \** pointer to access it. *\[note:
we won't be discussing RTcmix instrument design here, please see the
[instrument design](instrumentdesign.html) tutorial.\]* To accomplish
this, you will need to declare a *WAVETABLE \** pointer (including the
appropriate "WAVETABLE.h" for the required C++ object definition):

``` 
       #include 
       #include 
#include "WAVETABLE.h"

// ... all of the RTcmix set-up and use for RTcmix *rrr ...

RTcmix *rrr;
WAVETABLE *theWave;

rrr = new RTcmix();
sleep(1);

rrr->cmd("load", 1, "WAVETABLE");

rrr->cmd("makegen", 7, 1.0, 24.0, 1000.0, 0.0, 1.0, 3.5, 1.0);
rrr->cmd("makegen", 6, 2.0, 10.0, 1000.0, 1.0, 0.4, 0.2);

theWave = (WAVETABLE *)rrr->cmd("WAVETABLE", 4, 0.0, 999.0, 20000.0, 440.0);
```

At this point, *theWave* can now be used to change the frequency of the
note (notice that we set the duration to 999.0 seconds so that it will
be making sound continuously:

``` 
theWave->changeFrequency(314.78);
theWave->changeFrequency(249.0);

// etc.
```

There are, however, some RTcmix scorefile commands (such as *cpspch*
that return numerical values instead of *Instrument \** pointers. We
decided not to figure out how to handle multiple return types, but
instead wrote a *cmdval()* method for the RTcmix object. This method
works the same way that *cmd()* does, except that it returns a
floating-point value after it executes.

``` 
RTcmix *rrr;
float freq;

rrr = new RTcmix();

freq = rrr->cmdval("cpspch", 1, 8.09);
```

will assign the value "440.0" to the *freq* variable.

With *cmd()* and *cmdval*, you can make use of all that the RTcmix
object has to offer. We have added a few 'shortcuts' that can make your
programming life a teeny bit easier. For instance,

``` 
rrr->printOn();
```

and

``` 
rrr->printOff();
```

will turn on and off <u>all</u> RTcmix output. You may want to place the
*printOff()* command directly after the *new RTcmix()* constructor to
prevent RTcmix printing output from within an application.

RTcmix commands that have no arguments may be called without the
*NPARAMS*:

``` 
avar = rrr->cmd("random");
```

Also note that *cmdval()* is not necessary for this 0-pfield scorefile
command.

## A note about RTcmix instrument loading

A little discussion of the *load* scorefile command in an embedded
application is necessary, though. When you bundle a finished
RTcmix-embedding application, you will need to include the dynamic
library files for the instruments you use (like "libWAVETABLE.so").
These are found in the "shlib/" subdirectory of RTcmix, or (as in the
case of the *changeFrequency()* method added to WAVETABLE above) you
will want to include an instrument library that you have compiled.
Looking closely at the documentation for the
[load](../reference/scorefile/load.html) scorefile command, notice that
it can use absolute or relative pathnames to find the dynamic library
for loading. If you had created a "libMYWAVETABLE.so" instrument
library, then you could simply keep it in the same directory with your
finished RTcmix-embedding app, and call the *load* command like this:

``` 
rrr->cmd("load", 1, "./libMYWAVETABLE.so");
```

and it should work just fine. Alternatively, you could build an
installer that would place "libMYWAVETABLE.so" into some common
directory, like "/usr/local/lib" and use:

``` 
rrr->cmd("load", 1, "/usr/local/lib/libMYWAVETABLE.so");
```

*load* should also work fine in this case.

## An additional utility function

Including the RTcmix object in your application also loads in a function
that isn't part of the RTcmix object 'proper', but is very useful for
digital audio applications. The function
[RTtimeit()](../reference/interface/RTtimeit.html) will allow you to
easily set up a fairly well-timed, repeating call to another function.
The *RTtimeit()* function takes two arguments, the first is a
floating-point number that is the number of seconds between each call to
the second argument, a pionter to a void-returning function.

As a demonstration of this, suppose you wrote a function called
*gonotes()* that generated a burst of 8 notes, and you wanted this burst
to occur every 2.4 seconds. In your embedding application the
*gonotes()* function would be declared:

``` 
void *gonotes();
```

and the *RTtimeit()* call to make *gonotoes()* fire every 2,4 seconds
would be:

``` 
RTtimeit(2.4, (sig_t)gonotes);
```

The *RTtimeit()* can be called with a different timing value for
*gonotes()* at any point, including within the *gonotes()* function
itself. Setting the timing value in *RTtimeit()* to 0.0 should turn off
the repeating function calls. *\[note:* RTtimeit() *uses the Unix
SIGALRM signal, and will 'wake up' any processes also using SIGALRM
(like* sleep()*. You may need to place* sleep()*'s in a* while *loop.
See the "RTcmix/imbed/arpeggiate" program for an example of this.\]*

## Makefiles and Embedded *RTcmix*

Although Makefiles can be weird and esoteric things, creating one to
compile an embedded RTcmix application shouldn't be too difficult...
assuming, of course, that you have a Makefile that will indeed build the
application itself (i.e. without the RTcmix addition).

Here is a Makefile to compiled a C++/OpenGL program called "fredspace",
with no RTcmix inclusions:

``` 
XINCS = -I/usr/X11R6/include/
XFLAGS =  -L/usr/X11R6/lib/ -lXext -lX11 -lGL -lGLU -lm -laux

fredspace: fredspace.o
      g++ -o fredspace fredspace.o $(XFLAGS)

fredspace.o: fredspace.C
      g++ -c fredspace.c $(XINCS)
```

If we modify the "fredspace.C" program to use the RTcmix object, we only
need a few changes to the Makefile to compile it:

``` 
include /usr/local/src/RTcmix/makefile.conf

XINCS = -I/usr/X11R6/include/
XFLAGS =  -L/usr/X11R6/lib/ -lXext -lX11 -lGL -lGLU -lm -laux

# So main() will declare RTcmix globals
GLOBALS = $(ARCHFLAGS) -I$(CMIXDIR)/H
IMBCMIXOBJS += $(PROFILE_O)

fredspace: fredspace.o
      g++ -o fredspace fredspace.o $(GLOBALS) $(DYN) $(XFLAGS) $(IMBCMIXOBJS) $(LDFLAGS)

fredspace.o: fredspace.C
      g++ $(GLOBALS) -c fredspace.c $(XINCS)
```

The first change to the Makefile, the line:

``` 
include /usr/local/src/RTcmix/makefile.conf
```

will set up the Makefile with compiler flags and definitions that are
specific to your operating system and RTcmix. We are assuming here that
RTcmix is installed in "/usr/local/src/RTcmix". The file "makefile.conf"
in the top-level RTcmix directory contains these defintions. etc.

The Makefile lines:

``` 
# So main() will declare RTcmix globals
GLOBALS = $(ARCHFLAGS) -I$(CMIXDIR)/H
IMBCMIXOBJS += $(PROFILE_O)
```

are probably somewhat redundant, but they guarantee that appropriate
compiled information is set for the Makefile.

Finally, including the Makefile variables *$(GLOBALS) $(DYN)
$(IMBCMIXOBJS)* and *$(LDFLAGS)* in the main *fredspace* target and
including *$(GLOBALS)* in the *fredspace.o* object target should allow
the compiler/linker to find all of the library and header files it needs
to build the application successfully. *\[note: Except for* $(GLOBALS)*,
all of the Makefile target flags listed are from the RTcmix file
"makefile.conf".\]*

This approach to creating a Makefile is pretty generic, "old-style"
Unix, but it shouldn't be too difficult to use this as a guide to set up
various compiler-environments and project-builder applications for
embedded RTcmix applications. In addition to some of the specific flags
and \#defines found in the "makefile.conf" file, it will be important to
put the "RTcmix/H" directory on the search path for header files, and
the "RTcmix/lib" directory in the search path for the "genlib.a"
library. Also, the files "RTcmix/sys/cmix.o" and
"RTcmix/Minc/inbRTcmix.o" will need to be linked in order to build the
final executable application.

## A note about compiling RTcmix for embedded use

In certain circumstances, it isn't good to have RTcmix exit when a
scorefile error occurs. In the main "RTcmix/makefile.conf" file, the
following entry can be modified:

``` 
# Comment this out to set the die() function so that it will not exit on
# encountering an error (you may want to do this if you are using
# RTcmix in the context of another application where you don't want
# to terminate the application because of an RTcmix error
CMIX_FLAGS += -DEXIT_ON_ERROR
```

by commenting out the *CMIX\_FLAGS* line:

``` 
#CMIX_FLAGS += -DEXIT_ON_ERROR
```

Recompiling RTcmix will set it so that instruments returning the value
*DONT\_SCHEDULE* (defined in "RTcmix/rtstuff/rtdefs.h") from their
*::init()* member functions will <u>not</u> be placed on the execution
queue, and print a warning message without exiting. This will keep
RTcmix from halting the execution of the calling application.  
  
  
Brad Garton  
August, 2003
