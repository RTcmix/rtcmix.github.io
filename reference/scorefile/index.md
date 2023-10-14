---
title: Scorefile Commands and Functions
layout: ref
---

# Scorefile Commands and Functions

RTcmix scorefile commands are used to set up and control the execution
of RTcmix. They also include a number of utility commands for
algorithmic music-creation work using the default [Minc](Minc.html)
scorefile parser. RTcmix commands generally return values that are type
"double".

Although these commands are designed to be used within an RTcmix
scorefile, they can also be used as part of an embedded application,
working in conjunction with the [RTcmix
object](../interface/RTcmix-embed.html).

**Arrange by: [Topic](#sort_topic)  · 
[Alphabetical](#sort_alphabetical)**

<span id="topic" style="display:block"> </span>

## General

  - [Minc](Minc.html) &mdash; overview of the default Minc scorefile parsing
    language
  - [rtsetparams](rtsetparams.html) &mdash; set basic audio parameters
    (sampling rate, number of channels, etc.)
  - [len](len.html) &mdash; return the length of the argument
  - [load](load.html) &mdash; load an instrument into RTcmix for use
  - [rtinput](rtinput.html) &mdash; open a soundfile or input device for
    reading
  - [rtoutput](rtoutput.html) &mdash; open a soundfile for writing
  - [set\_option](set_option.html) &mdash; turn on and off various RTcmix
    options
  - [bus\_config](bus_config.html) &mdash; configure and connect the inputs
    and outputs of RTcmix instruments
  - [reset](reset.html) &mdash; set the control function/envelope update rate
  - [control\_rate](reset.html) &mdash; set the control function/envelope
    update rate
  - [include](include.html) &mdash; embed one scorefile within another
  - [index](index_command.html) &mdash; return the index of an item in a list
  - [exit](exit.html) &mdash; terminate RTcmix
  - [f\_arg](f_arg.html) &mdash; return a floating-point argument to the CMIX
    command
  - [i\_arg](f_arg.html) &mdash; return an integer argument to the CMIX
    command
  - [n\_arg](f_arg.html) &mdash; return the number of arguments to the CMIX
    command
  - [s\_arg](f_arg.html) &mdash; return a string argument to the CMIX command
  - [print](print.html) &mdash; print values
  - [makeinstrument](makeinstrument.html) - create handle for CHAIN
  - [printf](printf.html) &mdash; print formatted values
  - [print\_off](print_off.html) &mdash; stop printing of RTcmix stats to the
    screen
  - [print\_on](print_on.html) &mdash; allow printing of RTcmix stats to the
    screen
  - [stringify](stringify.html) &mdash; return a pointer to a string argument
  - [system](system.html) &mdash; execute Terminal or Shell commands from
    within RTcmix
  - [type](type.html) &mdash; return the Minc data-type of its argument

## Soundfile/Audio Information

  - [CHANS](CHANS.html) &mdash; return \# of channels of input soundfile
  - [DUR](DUR.html) &mdash; return duration of input soundfile
  - [LEFT\_PEAK](PEAK.html) &mdash; return peak amplitude for left channel of
    input soundfile
  - [PEAK](PEAK.html) &mdash; report peak amplitude for input soundfile
  - [RIGHT\_PEAK](PEAK.html) &mdash; return peak amplitude for right channel
    of input soundfile
  - [SR](SR.html) &mdash; return sampling rate of input soundfile
  - [filechans](filechans.html) &mdash; return \# of channels of a named
    soundfile
  - [filedur](filedur.html) &mdash; return duration of a named soundfile
  - [filepeak](filepeak.html) &mdash; return peak amplitude of a named
    soundfile
  - [filesr](filesr.html) &mdash; return sampling rate of a named soundfile
  - [getamp](getamp.html) &mdash; retrieve amplitude value from an LPC
    analysis file
  - [getpch](getpch.html) &mdash; retrieve pitch value from an LPC analysis
    file

## Math/Data/Numerical Conversion

  - [abs](abs.html) &mdash; absolute value of argument
  - [ampdb](ampdb.html) &mdash; convert decibels to amplitude
  - [dbamp](dbamp.html) &mdash; convert amplitude to decibels
  - [boost](boost.html) &mdash; return amplitude multiplier for stereo
    compensation
  - [log](log.html) &mdash; return the log10 of the argument
  - [pow](pow.html) &mdash; raise (exponentiate) one argument by the other
  - [max](max.html) &mdash; return maximum value of params
  - [min](min.html) &mdash; return minimum value of params
  - [mod](mod.html) &mdash; return result of modulus operation
  - [round](round.html) &mdash; return nearest integer to the argument
  - [trunc](trunc.html) &mdash; truncate a value to integer portion
  - [wrap](wrap.html) &mdash; modulus-like command
  - [translen](translen.html) &mdash; calculate duration from a transposition
    value (*oct.pc*)

## Pitch-Specification Conversion

  - [cpslet](cpslet.html) &mdash; convert from text-string note letter
    representation to frequency (Hz)
  - [cpsmidi](cpsmidi.html) &mdash; convert from midi note \# to frequency
    (Hz)
  - [cpsoct](cpsoct.html) &mdash; convert from linear octaves to Hz
    specification
  - [cpspch](cpspch.html) &mdash; convert from *oct.pc* to Hz specification
  - [midipch](midipch.html) &mdash; convert *oct.pc* to midi note \#
  - [octcps](octcps.html) &mdash; convert from Hz specification to linear
    octaves
  - [octlet](octlet.html) &mdash; convert from text-string note letter
    representation to linear octaves
  - [octmidi](octmidi.html) &mdash; convert from midi note \# to linear
    octaves
  - [octpch](octpch.html) &mdash; convert from *oct.pc* to linear octaves
  - [pchcps](pchcps.html) &mdash; convert from Hz to *oct.pc* specification
  - [pchlet](pchlet.html) &mdash; convert from text-string note letter
    representation to *oct.pc* specification
  - [pchmidi](pchmidi.html) &mdash; convert from midi note \# to *oct.pc*
  - [pchoct](pchoct.html) &mdash; convert from linear octaves to *oct.pc*

## Random-Number Commands

  - [irand](irand.html) &mdash; return a random number within a specified
    range
  - [pickrand](pickrand.html) &mdash; return a random choice from a specified
    set of numbers
  - [pickwrand](pickwrand.html) &mdash; return a weighted random choice from
    a specified set of numbers
  - [rand](rand.html) &mdash; return a random number between -1.0 and 1.0
  - [random](random.html) &mdash; return a random number between -0.0 and 1.0
  - [srand](srand.html) &mdash; set the initial seed value for the RTcmix
    psuedo-random number generator
  - [trand](trand.html) &mdash; return a random integer within a specified
    range
  - [spray\_init](spray_init.html) &mdash; initialize a random number "spray
    can"
  - [get\_spray](get_spray.html) &mdash; retrieve a value from a ["spray
    table"](spray_init.html)

## Envelope/Control (Function tables, Interface connections, PField variables)

  - [makeconnection](makeconnection.html) &mdash; create PField connection to
    an interface or device
  - [maketable](maketable.html) &mdash; create a table using the table-handle
    scheme:
  - [modtable](modtable.html) &mdash; modify a table given a table-handle:
  - [add](add.html) &mdash; add to a table-handle table given constant or
    another table-handle table
  - [copytable](copytable.html) &mdash; copy and optionally resize a table
    from a table-handle
  - [div](div.html) &mdash; divide the values in a table-handle table by a
    constant or another table-handle table
  - [dumptable](dumptable.html) &mdash; print the contents of a table given a
    table-handle
  - [makeLFO](makeLFO.html) &mdash; set up a low-frequency oscillator for
    PField control use
  - [makeconverter](makeconverter.html) &mdash; use a conversion program to
    change PField data format
  - [makefilter](makefilter.html) &mdash; establish a filter to transform
    PField variable data:
  - [makemonitor](makemonitor.html) &mdash; display or record PField control
    data
  - [makerandom](makerandom.html) &mdash; set up a periodic random-number
    generator for PField control use
  - [mul](mul.html) &mdash; multiply a table given a table-handle by a
    constant or another table-handle table
  - [plottable](plottable.html) &mdash; plot the contents of a table given a
    table-handle
  - [samptable](samptable.html) &mdash; return a value (specific or
    interpolated) from a table given a table-handle
  - [sub](sub.html) &mdash; subtract values from a table-handle table given a
    constant or another table-handle table
  - [tablelen](tablelen.html) &mdash; return the size of a table from a
    table-handle

<span id="alphabetical" style="display:none;"> </span>

  - [Minc](Minc.html)
  - [CHANS](CHANS.html)
  - [DUR](DUR.html)
  - [SR](SR.html)
  - [abs](abs.html)
  - [add](add.html)
  - [ampdb](ampdb.html)
  - [boost](boost.html)
  - [bus\_config](bus_config.html)
  - [control\_rate](reset.html)
  - [copytable](copytable.html)
  - [cpslet](cpslet.html)
  - [cpsmidi](cpsmidi.html)
  - [cpsoct](cpsoct.html)
  - [cpspch](cpspch.html)
  - [div](div.html)
  - [dumptable](dumptable.html)
  - [dbamp](dbamp.html)
  - [exit](exit.html)
  - [f\_arg](f_arg.html)
  - [filechans](filechans.html)
  - [filedur](filedur.html)
  - [filepeak](filepeak.html)
  - [filesr](filesr.html)
  - [getamp](getamp.html)
  - [getpch](getpch.html)
  - [get\_spray](get_spray.html)
  - [i\_arg](f_arg.html)
  - [include](include.html)
  - [index](index_command.html)
  - [irand](irand.html)
  - [len](len.html)
  - [load](load.html)
  - [log](log.html)
  - [makeconnection](makeconnection.html)
  - [makeconverter](makeconverter.html)
