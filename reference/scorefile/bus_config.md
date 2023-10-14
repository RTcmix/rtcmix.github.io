---
title: bus\_config()
layout: ref
---

## bus\_config

Configure input and output buses for an instrument.

-----

### Synopsis

**bus\_config**(*instrument\_name* \[, *input\_bus\_range*,
*input\_bus\_range*, ... \], *output\_bus\_range* \[,
*output\_bus\_range*, ... \])

Parameters inside the \[brackets\] are optional.

*(note: See the [bus\_config tutorial](../../tutorials/bus_config.html)
for more information on the command.)*

-----

### Description

RTcmix lets you direct an instrument to receive input from, and deliver
output to, other instruments, as well as sound files and the audio
device. For example, an instrument can read from a sound file and send
its output to another instrument, which in turn can send its output to
the audio device so that you can hear it. You can create a chain of
sound-processing instruments in this manner.

This kind of signal routing is made possible by an "aux bus" -- an
intermediate path for sound to take inside RTcmix. (The name comes from
aux buses on analog mixers, but the analogy isn't perfect.)

You can also make use of multi-channel audio hardware with RTcmix. Most
instruments work only in stereo, but you can route their outputs to any
pair of channels.

You configure the signal-routing path using the **bus\_config** script
command. This associates an instrument with a set of input and output
buses. Every call to the instrument following its **bus\_config** uses
this bus configuration. You can change the configuration for subsequent
instances of the same instrument by calling **bus\_config** again.

-----

### Arguments

  - *instrument\_name*  
      
    Name of the instrument whose buses you want to configure, in
    double-quotes.
    
    For instrument families, this must be the name of the individual
    instrument, not the name of the family. For example,
    [IIR](../instruments/IIR.html) is the name of a family, and
    [INPUTSIG](../instruments/IIR.html#INPUTSIG), and
    [BUZZ](../instruments/IIR.html#BUZZ), and
    [IINOISE](../instruments/IIR.html#IINOISE) and and
    [PULSE](../instruments/IIR.html#PULSE) are names of individual
    instruments in the family.

  - *input\_bus\_range*  

  - *output\_bus\_range*  
      
    A "bus range" is a description of a bus type, and one or more
    channels on that bus. The bus range must be in double-quotes. There
    can be any number of bus ranges in a **bus\_config** call, as long
    as they all make sense.
    
    There are three bus types:
    
      - *in* - input from file or audio device  
    
      - *out* - output to file or audio device  
    
      - *aux* - intermediate bus, used to connect instruments  
    
    ("Audio device" just refers to the hardware that handles sound I/O,
    such as a sound card on a PC or the built-in hardware on a Mac.)
    
    You combine the bus type with a channel number, or a range of
    channels specified by two channel numbers separated by a hyphen.
    Channels are numbered from zero.
    
    Since an aux bus can function as either an input or an output, you
    have to say which.
    
    Valid bus ranges include "in 0", "out 0-1", "aux 0 in", "aux 4-5
    out".
    
    There must always be at least one *output\_bus\_range*. Instruments
    that require input must also have at least one *input\_bus\_range*.
    
    Specifying an output bus that doesn't exist is an error. The number
    of output buses is set using [rtsetparams](rtsetparams.html).

-----

### Examples

``` 
   bus_config("WAVETABLE", "out 0-1")
```

This sends the output of subsequent
[WAVETABLE](../instruments/WAVETABLE.html) calls to the first two
channels of the audio device, or to a file, if [rtoutput](rtoutput.html)
has been called.

``` 
   bus_config("WAVETABLE", "out 0", "out 1")
```

This means the same thing.

``` 
   bus_config("WAVETABLE", "out 3", "out 7")
```

Output goes to channels 3 and 7 (counting from zero).

``` 
   bus_config("WAVETABLE", "aux 0-1 out")
```

Output goes to aux buses 0 and 1. Unless another instrument reads these
and sends output to the audio device, you'll hear nothing.

``` 
   bus_config("INPUTSIG", "aux 4-5 in", "out 0-1")
```

Input comes from aux buses 4 and 5; output goes to channels 0 and 1 of
the audio device, or a file opened with [rtoutput](rtoutput.html).

Notice that the instrument name is not the family name
([IIR](../instruments/IIR.html)), but the name of the function you call
to make sound.

``` 
   bus_config("FILTSWEEP", "aux 2 in", "aux 5 in",
                           "aux 1 out", "aux 7 out")
```

Reads from aux buses 2 and 5; writes to aux buses 1 and 7.

``` 
   bus_config("STEREO", "in 0", "out 0-1")
```

You'd think this would read from channel 0 of an input file, even if the
file has more channels. But RTcmix insists on reading all channels from
a file. (See below for more about this inconsistency.) If the last
[rtinput](rtinput.html) call gives the source as "MIC", then the
instrument does read just from the first channel.

``` 
   bus_config("WAVETABLE", "out 3")
   WAVETABLE(start, dur, amp, freq)
   bus_config("WAVETABLE", "out 7")
   WAVETABLE(start, dur, amp, freq)
```

The two WAVETABLE notes are identical and play at the same time, except
the first goes to channel 3 and the second goes to channel 7.

For examples of instrument chaining, see the scripts in
RTcmix/docs/sample\_scos\_3.0/.

-----

### NOTES

If you don't call bus\_config before using an instrument, you'll get a
default configuration roughly equal to:

``` 
   bus_config("FOO", "in 0-1", "out 0-1")
```

There are 16 aux buses (though this can be changed by recompiling your
copy of RTcmix -- see MAXBUS in H/bus.h).

-----

### LIMITATIONS

## An inconsistency for file input

When reading from a file, RTcmix ignores the bus numbers you use with
the "in" bus in the call to **bus\_config**. Instead, it reads all the
channels in the file. This behavior may change in a future version.

For more detail, see *RTcmix/docs/README.bus\_config*.

## Constraints on reading from an aux bus

A call to an instrument reading from a real-time audio source, whether
it be an aux bus or microphone input, **must** use an inskip of zero.
You can't read a segment of sound that hasn't happened yet\!

Also, some instruments just won't work well (or at all) when reading
from a real-time source. [TRANS](../instruments/TRANS.html) is an
example of this. When it transposes up an octave, it has to consume two
samples for every output sample. So it will always be left gasping for
more input.

For instruments that need to look into the future by a constant
interval, like [COMPLIMIT](../instruments/COMPLIMIT.html) <span>with
non-zero lookahead, the instrument merely delays its output enough to
"catch up" with its input. Such an instrument will work with input from
an aux bus.</span>

## Multiple bus types

Currently it is not possible to read from both an "in" bus and an "aux"
bus in the same instrument. Nor is it possible to write to both an "out"
bus and an "aux" bus.

-----

### See Also

[rtsetparams](rtsetparams.html), [rtinput](rtinput.html),
[rtoutput](rtoutput.html), [set\_option](set_option.html)
