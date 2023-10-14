---
title: makeconnection()
layout: ref
---

## makeconnection

Establish a control connection to an external device or process.

-----

### Synopsis

pfield = **makeconnection**(*"connection\_type"*\[, *arg1, arg2, ...*\])

Parameters inside the \[brackets\] are optional.

-----

### Description

**makeconnection** is a utility command used to associate data coming
from "outside" RTcmix with a *pfield-handle* variable. This variable can
then be used to deliver values to an Instrument during scorefile
execution. This is very similar to the mechanism used by the
[maketable](maketable.html) scorefile command to deliver table data to
Instruments through table-handle variables. Indeed, *table-handles* and
*pfield-handles* are nearly identical in terms of scorefile use and
behavior. The main difference is that *table-handle* variables are
accessing values from a previously-created table, while *pfield-handle*
variables are funneling (in real-time) data from some external process
of device into RTcmix.

This is almost the inverse of how the ['embedded' RTcmix
object](../interface/RTcmix-embed.html) works. The RTcmix object can be
used inside another application, with all interface tasks mediated
through the "wrapping" application. RTcmix is a subsidiary object in
this case. **makeconnection** allows an interface application to be
developed within RTcmix, with the interface code existing as a [PField
class](../interface/PField.html) object. Interfaces can then be
"connected" to control different Instrument/note parameters through a
*pfield-handle*.

Because device and interface characteristics can vary widely, the syntax
of a **makeconnection** command will also be relatively unique for
particular interfaces. At present, three different *connection\_type*
interfaces are included in RTcmix.

Many of the arguments for **makeconnection** may themselves also be
pfield-handles.

-----

### Arguments

  - *"connection\_type"*  
      
    This string value (i.e., enclosed in "double quotes" in the
    scorefile) determines the device or interface that will be
    associated with the *pfield-handle* returned by **makeconnection**.
    Currently three different *[connection\_types](#CONNECTION_TYPES)*
    are included in the RTcmix distribution, *["mouse"](#mouse)*,
    *["midi"](#midi)*, *["datafile"](#datafile)*, and
    *["inlet"](#inlet)* (in the Max/MSP
    [rtcmix\~](../../rtcmix_/index.html) object distribution). Each
    *connection\_type* used loads a library containing the interface
    code. These libraries should be in the <u>RTcmix/shlib/</u>
    directory or some other directory on the RTcmix dynamic-loading
    search path. The default RTcmix installation procedure should set
    this up properly.
  - <span id="item_arg1_arg2_etc" class="internallink">*arg1, arg2,
    ...*</span>  
      
    Any number of arguments that are needed to set up the
    interface/device connection. These depend on the connection type.
    See documentation for each type below.

-----

## <span id="CONNECTION_TYPES" class="internallink">Connection Types</span>

  - <span id="mouse" class="internallink">*mouse*</span>  
      
    This establishes a PField variable that will draw data from the
    mouse screen position. The syntax is:
    This will cause a window to be displayed, and movement of the mouse
    relative to that window will be tracked. An executing note using the
    *pfield* variable as an Instrument parameter will draw data from the
    position of the mouse to provide the values for that parameter. More
    than one parameter or Instrument may be controlled simultaneously,
    depending on how the connections are specified in the scorefile. The
    other arguments determine how the mouse data will be set and
    interpreted:
      - *"axis"* &mdash; this string argument can be either *"x"* or *"y"*
        and sets which axis will be read for data from the mouse.  
          
      - *min* &mdash; the value when the mouse x or y position is at 0 on the
        display.  
          
      - *max* &mdash; the maximum value reached as the mouse is moved to the
        maximum set position on the display.
        Note that if *min* and *max* are inverted, the mouse will
        deliver values 'backwards'.  
          
      - *default* &mdash; the initial value used for the *pfield-handle*.  
          
      - *lag* &mdash; amount of smoothing for value stream \[percent:
        0-100\]. This applies a simple filter to the incoming data to
        prevent values from jumping abruptly, potentially causing
        discontinuity glitches in executing notes.  
          
      - *"prefix"* &mdash; an optional string value that will be used to
        label reporting in the window used by the mouse.  
          
      - *"units"* &mdash; an optional string value that will be displayed in
        the mouse window to report mouse location data.  
          
      - *precision* &mdash; this optional number determines how many digits
        after the decimal point will be displayed in the mouse window to
        report mouse location data.

  - <span id="midi" class="internallink">*midi*</span>  
      
    This establishes a PField variable that will accept data from an
    attached MIDI interface. The syntax is:


    An executing note using the *pfield* variable as an Instrument
    parameter will draw data from a MIDI controller attached to a MIDI
    interface to provide the values for that parameter. As with the
    ["mouse"](#mouse) connection type, more than one parameter or
    Instrument may be controlled simultaneously, depending on how the
    connections are specified in the scorefile. The other arguments
    establish which MIDI channel and parameter will be monitored and
    determine how the MIDI data will be interpreted:

      - *min* &mdash; the value when the MIDI controller delivers 0.  
    
      - *max* &mdash; the maximum value reached when the MIDI controller is
        at the maximum of the range (usually 127).
        
        Note that if *min* and *max* are inverted, the mouse will
        deliver values 'backwards'.  
    
      - *default* &mdash; the initial value used for the *pfield-handle*.  
    
      - *lag* &mdash; amount of smoothing for value stream \[percent:
        0-100\]. This applies a simple filter to the incoming data to
        prevent values from jumping abruptly, potentially causing
        discontinuity glitches in executing notes.  
    
      - *chan* &mdash; the MIDI channel to use (1-16).  
    
      - *type* &mdash; the MIDI parameter to monitor. This string value will
        deliver data as follows:
        
            value                     MIDI data
            ---------------------------------------------------------------
            "noteonpitch"             MIDI note # for noteon events
            "noteonvel"               MIDI velocity for noteon events
            "noteooffpitch"           MIDI note # for noteoff events
            "noteoffvel"              MIDI velocity for noteoff events
            "cntl"                    use "subtype" to specify
                                         which MIDI controller to use
            "prog"                    MIDI program change #
            "bend"                    MIDI pitch-bend data
            "chanpress"               MIDI channel change value
            "polypress"               use "subtype" somehow
        
      - *subtype* &mdash; if *type* is set to *"cntl"*, then *subtype* is
        used to specify which MIDI controller is monitored. This can be
        set using a string symbol or the corresponding MIDI controller
        \#:
        
            subtype                   MIDI controller
            ---------------------------------------------------------------
            "mod"                     modulator wheel
            "foot"                    foot pedal
            "breath"                  breath controller
            "data"                    data slider
            "volume"                  volume slider
            "pan"                     pan silder
        
        If *type* is set to *"polypress"*, then somehow a MIDI note
        number is used for something.

  - <span id="datafile" class="internallink">*datafile*</span>  
      
    This establishes a PField variable that will read control data from
    an existing datafile, probably created using the
    [makemonitor](makemonitor.html#datafile) scorefile command. The
    syntax is:

    This will open the file *filename* for reading. An executing note
    using the *pfield* variable as an Instrument parameter will draw
    data from the file to provide the values for that parameter. More
    than one parameter or Instrument may be controlled simultaneously,
    depending on how the connections are specified in the scorefile. The
    arguments determine how the file will be opened and read:

      - *"filename"* &mdash; this string argument is a relative or absolute
        pathname to an existing file.  
          
      - *lag* &mdash; the amount of smoothing for value stream \[percent:
        0-100\]. This applies a simple filter to the incoming data to
        prevent values from jumping abruptly, potentially causing
        discontinuity glitches in executing notes.  
          
      - *skiptime* &mdash; the time in seconds to skip before reading data
        file, prior to applying *timefactor*. This is optional; default
        is 0.  
          
      - *timefactor* &mdash; this scales (expands or contracts) the time it
        takes to read the file: 1 means use the same amount of time it
        took to create the file; 2 means take twice as long to play the
        file data; 0.5 means take half as long; etc. This argument is
        optional; if used, *skiptime* must be given. Default for
        *timefactor* is 1.0.  
          
      - *lag* &mdash; amount of smoothing for value stream \[percent:
        0-100\]. This applies a simple filter to the incoming data to
        prevent values from jumping abruptly, potentially causing
        discontinuity glitches in executing notes.  
          
      - *filerate* &mdash; an optional parameter for datafiles with no file
        'header' containing information relevant to how the data in the
        file is interpreted. If this parameter is specified, then the
        subsequent *format* and *swap* parameters should also be
        present. *filerate* determines the control rate at which the
        data points from the file will be read. It is recommended that
        this be significantly less than the control rate (maybe 1/5 as
        fast) in order to 'thin' the file data a bit. This rate is the
        number of updates/second, and the overall control rate is set by
        the [reset](reset.html) scorefile command. The default overall
        control rate is 1000 updates/second.  
          
      - *format* &mdash; an optional string value used for datafiles with no
        file 'header'. This determines the format of the data in the
        file. Acceptable values are *"double", "float", "int64",
        "int32", "int16"* or *"byte".* This parameter is used in
        conjunction with the *filerate* and *swap* parameters.  
          
      - *swap* &mdash; this optional number sets whether or not byte-swapping
        should be employed when reading files with no file 'header'. A
        value of 0 sets no swapping; 1 will engage swapping. This
        parameter is used in conjunction with the *filerate* and
        *format* parameters.

  - <span id="inlet" class="internallink">*inlet*</span> (Max/MSP
    [rtcmix\~](../../rtcmix_/index.html) only)  
      
    This establishes a PField variable that will read data coming from
    an 'inlet' on the Max/MSP [rtcmix\~](../../rtcmix_/index.html)
    object. The syntax is:
    An executing note using the *pfield* variable as an Instrument
    parameter will draw data from a max-patch through one of the
    *rtcmix\~* object inlets. As with the ["mouse"](#mouse) and
    ["midi"](#mouse) connection types, more than one parameter or
    Instrument may be controlled simultaneously, depending on how the
    connections are specified in the scorefile.
      - *inlet\_number* &mdash; which inlet will be read for this
        *pfield-handle* variable.  
          
      - *default* &mdash; the initial value used for the *pfield-handle*.
    Please refer to the *rtcmix\~* object help-patch for more
    information on how this connection type is used.

-----

### Examples

``` 
   pan = makeconnection("mouse", "x", 1, 0, .5, lag=50, "pan")
   deltime = makeconnection("mouse", "x", 0.0002, .5, .5, lag=90, "delay time", "", 5)
   feedback = makeconnection("mouse", "y", 0, 1, 0, lag=20, "feedback")
   DELAY(0, 0, 10, 1.0, deltime, feedback, 1.0, 0, pan)
```

This scorefile uses three different *pfield-handle* variables: *pan*,
*deltime* and *feedback*. *pan* and *deltime* are controlled by the 'x'
position of the mouse, scaled by the *min* and *max* values for each
(\[1, 0\], \[0.0002, 0.5\]). The *deltime* parameter is set with a high
*lag* percentage to prevent glitching as the delay-time of the
[DELAY](../instruments/DELAY.html) instrument is changed. The *feedback*
parameter tracks the 'y' position of the mouse, scaling values between 0
and 1. Notice that these *pfield-handle* variables function in the same
way that [maketable](maketable.html) table-handle variables do.

See the <u>RTcmix/docs/sample\_scores/dynamic\_insts/</u> subdirectory
in the RTcmix distrubition for more examples of different connection
types.

-----

### NOTES

The **makeconnection** command will dynamically-load the library
containing the interface code used for each connection type. For
example:

``` 
   makeconnection("mouse", ...)
```

will look for the dynamic library <u>libmouse.so</u>. As discussed
above, this library needs to be locatable along the RTcmix library
search path. The default RTcmix installation will compile and install
this library into the <u>RTcmix/shlib/</u> directory.

-----

### See Also

[maketable](maketable.html), [makeLFO](makeLFO.html),
[makerandom](makerandom.html), [makefilter](makefilter.html),
[makeconverter](makeconverter.html), [makemonitor](makemonitor.html)
