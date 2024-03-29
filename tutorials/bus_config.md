---
title: Using bus\_config
layout: ref
---

# Using bus\_config

RTcmix has the ability to route signals between instruments and to multiple outputs.

## Bus architecture

Long ago, all instruments had to add their output into one master mix bus.
Now you can tell an instrument to send its output into an intermediate
bus, called an "aux" bus, and to take its input from another aux bus.
This lets you create a chain of instruments. For example, you could send
the output of WAVETABLE into FLANGE, and then send the output of FLANGE
into REVERBIT. In addition to instrument chaining, RTcmix lets you make
full use of multi-channel audio cards. You can route STEREO to outputs 0
and 1, while routing FILTSWEEP to outputs 2 and 3.

## The bus\_config Minc function

There are three types of bus:

> "in" input from file or from real-time audio source (e.g., mic)  
> "aux" intermediate bus, functioning as either an input or an
> outputh2  
> "out" output to a file or to the sound card

You configure the arrangement of buses with a new Minc function,
bus\_config. This associates an instrument name with a set of buses.
Every call to the instrument following its bus\_config uses this bus
configuration. It's possible to change this with another call to
bus\_config, which overrides the first (in the manner of makegen).

The first argument to bus\_config is the name of the instrument, in
double quotes. Following that are any number of bus specification
strings, which combine the name of a bus type ("in", "out") with a bus
number or range of numbers. Buses are numbered from zero. Since an aux
bus can be an input or an output, you have to say which. Some examples
(the quotes are obligatory):

> bus\_config("WAVETABLE", "out 0-1")
> 
> > This sends the output of subsequent WAVETABLE calls to the first two
> > channels of the sound card (or to a file, if rtoutput has been
> > called).
> 
> bus\_config("WAVETABLE", "out 0", "out 1")
> 
> > This means the same thing.
> 
> bus\_config("WAVETABLE", "out 3", "out 7")
> 
> > Output goes to channels 3 and 7 (counting from zero).
> 
> bus\_config("WAVETABLE", "aux 0-1 out")
> 
> > Output goes to aux buses 0 and 1. Unless another instrument reads
> > these and sends output to the sound card, you'll hear nothing.
> 
> bus\_config("INPUTSIG", "aux 4-5 in", "out 0-1")
> 
> > Input comes from aux buses 4 and 5; output goes to channels 0 and 1
> > of the sound card (or rtoutput file).
> > 
> > (Notice that the instrument name is not the family name ("IIR"), but
> > the name of the function you call to make sound. Same thing goes for
> > the STRUM family.)
> 
> bus\_config("FILTSWEEP", "aux 2 in", "aux 5 in", "aux 1 out", "aux 7
> out")
> 
> > Reads from aux buses 2 and 5; writes to aux buses 1 and 7.
> 
> bus\_config("STEREO", "in 0", "out 0-1")
> 
> > You'd think this would read from channel 0 of an input file, even if
> > the file has more channels. But RTcmix insists on reading all
> > channels from a file. (See below for more about this inconsistency.)
> > If the last rtinput call gives the source as "MIC", then the inst
> > does read just from the first channel.

There are 64 "aux" buses available for non-embedded builds (defined by
DEFAULT_MAXBUS in H/bus.h) and 16 for embedded. You set the maximum number
of "out" buses in the call to rtsetparams.

If you don't call bus\_config before using an instrument, you'll get a
default configuration roughly equal to:

> bus\_config("FOO", "in 0-1", "out 0-1")

Currently it is not possible to read from both an "in" bus and an "aux"
bus in the same instrument. Nor is it possible to write to both an "out"
bus and an "aux" bus. This restriction will be removed later.

## An inconsistency for file input

**Executive summary:**  
When reading from a file, RTcmix ignores the bus numbers you use with
the "in" bus in the call to bus\_config. Instead, it reads all the
channels in the file. This behavior may change in a future version.

**More detail:**  
"Aux" buses and real-time "in" buses (e.g., mic) are shared among all
instruments using them. Unlike these, sound file input buses are not
shared between instruments. They aren't really buses at all, but private
buffers. That's because instrument calls will often read from different
parts of a file, so it would make no sense for them to share file buses.
So we have an inconsistency in bus\_config. When you specify that an
instrument reads from "in" buses, and you make it read -- via rtinput --
from a file rather than a real-time source, then RTcmix will ignore the
bus numbers you give bus\_config, and will read all the channels in the
file. Note that many instruments have an pfield to let you say which
channel you want. It's possible that this scheme will change in the
future.

## Some constraints on reading from an aux bus

A call to an instrument reading from a real-time audio source, whether
it be an aux bus or microphone input, MUST use an inskip of zero. You
can't read a segment of sound that hasn't happened yet\!

Also, some instruments just won't work well (or at all) when reading
from a real-time source. TRANS is an example of this. When it transposes
up an octave, it has to consume two samples for every output sample. So
it will always be left gasping for more input. When it transposes down,
it has no way of keeping the ever-lengthening history of samples it
would need to generate output.

(For instruments that need to look into the future by a constant
interval, like COMPLIMIT with non-zero lookahead, the instrument merely
delays its output enough to "catch up" with its input.)

## For instrument designers

Please see "README.inst\_porting" in the source code for instructions on making your
instruments compatible with this version of RTcmix.

John Gibson  
Dave Topper
