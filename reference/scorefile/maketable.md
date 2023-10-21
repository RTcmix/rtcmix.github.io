---
title: maketable()
layout: ref
---

## maketable

Create a table to hold values for use by an instrument or
directly in a script.

-----

### Synopsis

table = **maketable**(*"table\_type"*, \[*"optional specifiers",*\]
*table\_size*, *arg1*\[, *arg2*, ... \])

Parameters inside the \[brackets\] are optional.

-----

### Description

**maketable** can be used for direct RTcmix Instrument envelope,
waveform and other control purposes. It returns a table "handle" which
can be stored in a scorefile variable. This "table-handle" can generally
operate like other RTcmix scorefile variables, i.e., it can be treated
arithmetically, a single table can control multiple aspects of an
Instrument (without the annoyance of maintaining the correct makegen
"table slot number"), and the syntax of the **maketable** command is
fairly transparent for understanding how a particular table is employed
in Instrument control.

**maketable** was introduced in RTcmix version 4.0, and you may
encounter some very old scores that use the outdated
[makegen](old/makegen.html) system. Many RTcmix instruments maintain
backwards-compatibility with this older system, but support for the use
of [makegen](old/makegen.html) constructs was dropped several years ago.

-----

### Arguments

  - *table\_type*  
      
    This string value (i.e., enclosed in "double quotes" in the
    scorefile) determines the kind of table that will be constructed.
    This specifier thus dictates how the rest of the arguments will be
    interpreted. Most of the older [makegen](old/makegen.html)
    table-creation functions are subsumed by the *"table\_type"*.
    
    See [below](#TABLE_TYPES) for a listing of current
    table-construction specifiers.

  - *optional\_specifiers*  
      
    These optional arguments determine global characteristics of how
    tables are constructed and subsequently accessed. Presently there
    are two sets of string specifiers:
    
      - *"norm"/"nonorm"* &mdash; this option determines whether or not the
        table will be normalized; with *"norm"* specified the table
        contents will be scaled to fit within the range 0.0&ndash;1.0 or
        -1.0&ndash;1.0. *"nonorm"* will turn off this scaling, allowing
        direct values to be specified when the table is constructed.  
          
        *"norm"* is the default.  
    
      - *"interp" / "interp2" / "nointerp"* &mdash; this option dictates how
        values will be read from the table. *"interp"* enables simple
        first-order linear interpolation, i.e. if a requested table
        value lies between two elements in the table, setting *"interp"*
        will generate an intermediate value based upon a linear
        interpolation of nearest sample values. For example, if the
        value at table location 314.15 were requested, the value
        returned would be 0.15 between the value at location 314 and the
        value at location 315. *"interp2"* uses a spline-interpolation
        scheme that may be more accurate for many curves, but more
        costly in terms of computation required. *"nointerp"* turns off
        interpolation; the values returned from the table will be
        'rounded-down' (truncated) to the nearest-lowest point in the
        table. For example, if the table value at location 149.78 were
        requested, the *"nointerp"* option would return the value stored
        at location 149.
        
        *"interp"* is the default.

  - <span id="item_table_size" class="internallink">*table\_size*</span>  
      
    The size of the table: how many numbers it stores. For most
    interpolated tables, you can use 1000 or 2000 here. You might want
    to use more or less, depending on the specific purpose. (For
    example, you might want 50 (and only 50) random numbers, so you
    would use the [random](#random) table type and set the table size to
    50.) There is no requirement that this size be a power of two.

  - <span id="item_arg1_arg2_etc" class="internallink">*arg1, arg2,
    ...*</span>  
      
    Any number of arguments that define the table. These depend on the
    table type. See documentation for each type below.
    
    Note that the number of arguments used to define the table is
    independent of the table size. So, depending on the table type, you
    can create a table with a size of 2000 using only one or two
    arguments.

-----

## <span id="TABLE_TYPES" class="internallink">Table Types</span>

  - <span id="textfile" class="internallink">*textfile*</span>  
      
    Fill a table with numbers read from a text file. The syntax is:

    ```
    table = maketable("textfile", size, "filename")
    ```
    The function loads as many as *size* numbers into the table. If
    there are not that many numbers in the text file, it zeros out the
    extra table values. The function reports one warning if at least one
    piece of text (delimited by whitespace) cannot be interpreted as a
    double. This means the file may contain any number of free text
    comments interspersed with the numbers, as long as the comments
    themselves do not contain numbers\!
    
    *filename* should be a string (i.e. enclosed in quotes). The
    filename can be a relative pathname to the directory where the
    scorefile was invoked, or it may be an absolute pathname within the
    filesystem.
    
    This replaces the older makegen function [gen 2](old/gen2.html),
    except that it does not return the length of the table created. This
    can be retrieved using the new [tablelen](tablelen.html) scorefile
    command.

  - <span id="soundfile" class="internallink">*soundfile*</span>  
      
    Fill a table with numbers read from a sound file. The syntax is:

    ```
    table = maketable("soundfile", size, "filename"[, duration[, inskip[, inchan]]])
    ```

    The *size* argument is ignored; set it to zero.
    
    *filename* is obligatory. As with the filename argument for the
    [textfile](#textfile) table, it is a string that can be a relative
    or an absolute pathname to a soundfile. All of the supported RTcmix
    soundfile types can be read (AIFF, WAV, NeXT, BSD, etc.).
    
    The other arguments are optional, but if an argument further to the
    right is given, all the ones to its left must also be given. These
    optional arguments are:
    
      - *filename* &mdash; the name of a sound file in any of the header
        types RTcmix can read; data formats: 16bit signed int, 32bit
        float, 24bit 3-byte signed int; either endian.  
          
      - *duration* &mdash; duration (in seconds) to read from file. If
        negative, its absolute value is the number of sample frames to
        read. If *duration* is missing, or if it's zero, then the whole
        file is read. Beware with large files &mdash; there is no check on
        memory consumption\!  
          
      - *inskip* &mdash; time (in seconds) to skip before reading, or if
        negative, its absolute value is the number of sample frames to
        skip. If *inskip* is missing, it's assumed to be zero.  
          
      - *inchan* &mdash; channel number to read (with zero as first channel).
        If *inchan* is missing all channels are read, with samples from
        each frame interleaved.
    
    This replaces the older makegen function [gen 1](old/gen1.html),
    except that it does not normalize the sample values. The arguments
    are slightly different also, and this does not return the number of
    frames read. The number of samples in the table (not frames\!) can
    be retrieved using the new [tablelen](tablelen.html) scorefile
    command.

  - <span id="literal" class="internallink">*literal*</span>  
      
    Fill a table with just the values specified as arguments. The syntax
    is:

    ```
    table = maketable("literal", "nonorm", size, n1, n2, n3, n4 ...)
    ```

    *n1, n2, n3,* etc. are the numbers that go into the table. The
    "nonorm" tag is recommended, unless you want the numbers to be
    normalized to \[-1.0, 1.0\] or \[0.0, 1.0\] (if no negative values
    are present).
    
    The function loads as many as *size* numbers into the table. If
    there are not that many number arguments, it sets the extra table
    values to zero. If *size* is zero, the table will be sized to fit
    the number arguments exactly. If one (or more) of the *n1, n2, ...*
    variables is an array created in the scripting language, then the
    array will be 'unpacked' into the table sequentially.
    
    This replaces the older makegen function [gen 2](old/gen2.html),
    except that it has the option to size the table to fit the number of
    arguments. It also does not return the number of elements in the
    table. This value can be retrieved using the new
    [tablelen](tablelen.html) scorefile command.

  - <span id="datafile" class="internallink">*datafile*</span>  
      
    Fill a table with numbers read from a data file. The syntax is:

    ```
    table = maketable("datafile", size, "filename"[, number_type])
    ```

    The function loads as many as *size* numbers into the table. This is
    very similar in function to the [textfile](#textfile) table type
    discribed above, except that the file contains 'raw' binary data,
    not text-formatted data. If there are not that many numbers in the
    file, it zeros out the extra table values. If *size* is zero, the
    table will be sized to fit the data exactly. Be careful if you use
    this option with a very large file: you may run out of memory\!
    
    *number\_type* can be any of *"float"* (the default), *"double",
    "int", "int64", "int32", "int16"* or *"byte"*. This just means that
    with the *"double"* type, for example, every 8 bytes will be
    interpreted as one floating point number; with the *"int16"* type,
    every 2 bytes will be interpreted as one integer; and so on. *"int"*
    is *"int32"* on a 32-bit platform and *"int64"* on a 64-bit
    platform. No byte-swapping is done.
    
    This replaces the older makegen function [gen 3](old/gen3.html),
    except that it has choices for the type of number, it has the option
    to size the table to fit the data, and it does not return the number
    of elements in the array. This value can be retrieved using the new
    [tablelen](tablelen.html) scorefile command.

  - <span id="curve" class="internallink">*curve*</span>  
      
    Fill a table with line or curve segments, defined by arguments. The
    syntax is:

    ```
    table = maketable("curve", size, time1, value1, curvature1,
            [ timeN-1, valueN-1, curvatureN-1, ] timeN, valueN)
    ```
    
    *curvature* controls the curvature of the each line segment from
    *valueN* to *valueN+1* over the interval *timeN* to *timeN+1*. The
    values for *curvature* are:
    
      - *curvature = 0* &mdash; makes a straight line  
          
      - *curvature \< 0* &mdash; makes a logarithmic (convex) curve; larger
        negative values make "sharper" curves  
          
      - *curvature \> 0* &mdash; makes a logarithmic (concave) curve; larger
        values make "sharper" curves
    
    The code was derived from *gen4* from the UCSD Carl package,
    described in F.R. Moore, *<u>Elements of Computer Music</u>.*
    
    This replaces the older makegen function [gen 4](old/gen4.html).
    Like gen 4, the time values specified in the table are relative to
    the actual duration that the table is being used. If the duration is
    equal to the last time point in the table, then the time values will
    align. Otherwise the table will be 'stretched' or 'compressed' to
    function in the total duration it is actually used.

  - <span id="expbrk" class="internallink">*expbrk*</span>  
      
    Fill a table using an exponential break-point function. The syntax
    is:

    ```
    table = maketable("expbrk", size, value1, npoints1, [ valueN-1, npointsN-1, ] valueN)
    ```
    
    This table type fills the table with exponential line-segments in
    the same way that the older makegen function [gen 5](old/gen5.html)
    did. All values must be \> 0. The *npointsN* arguments specify how
    many table elements are to be used in constructing the curve from
    one value to the next. All of the *npointsN* values should probably
    add up to equal the *size* of the table. If the *npointsN* values
    are less than the *size* of the table, then the last value will be
    repeated to fill the remaining table elements.

  - <span id="line" class="internallink">*line*</span>  
      
    Create a function table comprising straight line segments, specified
    by using any number of \[time, value\] pairs. The syntax is:

    ```
    table = maketable("line", size, time1, value1, [ timeN-1, valueN-1, ] timeN, valueN)
    ```
    
    This is like the [curve](#curve) syntax, except that straight
    line-segments are used to interpolate values over the time between
    the *valueN* points and the *curvature* values are therefore absent.
    Values can cross 0, and can indeed be 0. Times will be scaled by
    actual duration of use.
    
    This is nearly identical to several older makegen functions that are
    heavily used in RTcmix, [gen 6](old/gen6.html), [gen
    18](old/gen18.html) and [gen 24](old/gen24.html). There are some
    subtle internal differences in the *line* table type and these older
    makegen functions mainly having to do with how the final point in
    the table is reached &mdash; see the code if this is of concern.

  - <span id="linebrk" class="internallink">*linebrk*</span>  
      
    Fill a table using a linear break-point function. The syntax is:

    ```
    table = maketable("linebrk", size, value1, npoints1, 
                      [ valueN-1, npointsN-1, ] valueN)
    ```
    
    This table type fills the table with straight line-segments. The
    endpoints of the line segments are defined by each *valueN* to
    *valueN+1*arguments, and the "length" is determined by each
    *npointsN* argument between the two endpoints. This is very similar
    to how the [expbrk](#expbrk) table type works, except (of course)
    that lines are used to "connect the dots" instead of exponential
    segments. It is also identical in functionality to the older makegen
    [gen 7](old/gen7.html) function table routine. Crossing 0 and values
    of 0 are permitted. All of the *npointsN* values should probably add
    up to equal the *size* of the table. If the *npointsN* values are
    less than the *size* of the table, then the last value will be
    repeated to fill the remaining table elements.

  - <span id="spline" class="internallink">*spline*</span>  
      
    Fill a table with a spline curve, defined by at least three \[time,
    value\] points. The curve travels smoothly between the points, and
    all points lie on the curve. The syntax is:

    ```
    table = maketable("spline", size, ["closed",] curvature, time1, value1,
            time2, value2, [ timeN-1, valueN-1, ] timeN, valueN)
    ```
    
    The *curvature* argument controls the character of the slope between
    points. A value of 0 will produce a relatively flat curve
    connecting the points, higher values will produce more pronounced
    curved excursions.
    
    The optional specifier *"closed"* is another way of affecting the
    curvature. This interacts with the *curvature* value. To see the
    resulting shape of the curve, use the [plottable](plottable.html)
    command. Note that it is possible that the curve will loop outside
    of the area you expect, especially if the *"nonorm"* optional
    specifier is used.
    
    Adapted from *cspline* in the UCSD Carl package, described in F.R.
    Moore, *<u>Elements of Computer Music</u>.*

  - <span id="wave3" class="internallink">*wave3*</span>  
      
    Fill a table with one cycle of a waveform, composed of any number of
    partials. The partials are specified by a multiplier of the
    fundamental, amplitude and phase. The syntax is:

    ```
    table = maketable("wave3", size, partial_multiplier1, amplitude1, phase1
            [, ... partial_multiplierN, amplitudeN, phaseN])
    ```
    
    The *partial\_multiplier* adds a component to the waveform being
    constructed in the table. The fundamental frequency that the
    *partial\_multiplier* uses for the multiplication is set so that
    exactly one cycle of the fundamental will fit into the table. The
    *amplitude* and *phase* arguments that follow the
    *partial\_multiplier* then set the amplitude and phase of that
    partial. The *partial\_multiplier* does not have to be an integer
    (i.e., it can be fractional, like 1.78), but a fractionally-specified
    component of the waveform will be truncated to fit into the table
    determined by the basic fundamental frequency.
    
    *amplitude* generally operates on a scale of 0.0 &ndash; 1.0, but values
    higher than 1.0 are allowed. The default action will rescale the
    resulting waveform to -1.0 &ndash; 1.0. Negative amplitudes are also
    allowed, but this is identical to a positive amplitude with a
    180-degree phase shift.
    
    The *phase* argument is in degrees, 0.0 &ndash; 360.0. Negative phases
    and values \> 360 are allowed, but they are equivalent to
    corresponding phases between 0.0 &ndash; 360.0.
    
    What does all this mean? For example, the following use of
    **maketable**:

    ```
    table = maketable("wave3", 1000, 1, 1, 0)
    ```
    
	 will create a single cyle of a sine wave (the fundamental frequency
	 multiplied by 1.0, relative amplitude of 1.0, and phase shift of 0.0) and
	 store it &mdash; in digital form of course\! &mdash; in a 1000-point
	 table. Changing the above scorefile command to this:

    ```
    table = maketable("wave3", 1000, 1, 1, 90)
    ```
    
    will create a cosine wave &mdash; the sine wave is shifted in phase by 90
    degrees in the table. This specification:

    ```
    table = maketable("wave3", 1000, 2, 1, 0)
    ```
    
    builds a waveform in the table that has two complete cycles of a
    sine wave; the *partial\_multiplier* causes a partial to be built
    that is twice the fundamental frequency. And the following:

    ```
    table = maketable("wave3", 1000, 3.14, 1, 0)
    ```
    
    will place exactly 3.14 cycles of a sine wave into the table. These
    specifications can be mixed:

    ```
    table = maketable("wave3", 1000, 1, 1, 0, 2, 0.4, 90, 3.14, 0.01, 0)
    ```
    
    creates a waveform with the fundamental at relative amplitude 1, the
    second harmonic with an amplitude of 0.4 and a 90-degree phase
    shift, and a part-component of 3.14 with an amplitude of 0.01 also
    included.
    
    Most instruments that rely upon constructed wavetable (such as
    [WAVETABLE](../instruments/WAVETABLE.html) or
    [FMINST](../instruments/FMINST.html)) in RTcmix 4.0 have been
    modified to include an optional parameter that will allow the
    **maketable** waveform to be used. If this parameter is present, any
    makegen-constructed wavetables will be ignored.
    
    See the older makegen routine [gen 9](old/gen9.html) for more
    examples of how this waveform-constructing scheme works, as well as
    some plot images to show the effects of different \[partial,
    amplitude, phase\] combinations.

  - <span id="wave" class="internallink">*wave*</span>  
      
    Fill a table with one cycle of a waveform, composed of any number of
    harmonic partials. The partials are specified by a positional
    parameter corresponding to the relative amplitude of each partial
    included in the waveform construction. The syntax is:

    ```
    table = maketable("wave", size, partial1_amp [, partial2_amp, ... , partialN_amp])
    ```

    or

    ```
    table = maketable("wave", size, "wave_string")
    ```
    
    In the first use, each *partialN\_amp* relative amplitude will
    contribute the *Nth* harmonic at the specified amplitude to the
    composite waveform stored in the table. The phase of each harmonic
    is 0.0. Obviously non-harmonic (non-integer multiples of the
    fundamental) partials are not specifiable. *size* is the number of
    points allocated for the table.
    
    For example:

    ```
    table = maketable("wave", 2000, 1)
    ```
    
    will build a 2000-element table containing a single cycle of a sine
    wave (harmonic 1 at amplitude 1),

    ```
    table = maketable("wave", 2000, 0, 0, 1)
    ```
    
    will build a table containing three cycles of a sine wave (harmonic
    3 at amplitude 1), and

    ```
    table = maketable("wave", 2000, 1.0, 0.5, 0.1)
    ```
    
    will create a composite waveform with the amplitudes 1.0, 0.5, and
    0.1 of the first, second and third harmonics (respectively).
    
    The second use of the *wave* table type allows for specifying the
    waveform to be constructed by name. The following string values for
    the *waveform\_string* parameter are valid:

      - *"sine"* &mdash; sine waveform  
          
      - *"saw"* &mdash; sawtooth waveform, ramping down from 1.0 to -1.0  
          
      - *"sawX"* &mdash; sawtooth waveform, like *saw* using only the first *X* harmonics  
          
      - *"sawdown"* &mdash; sawtooth waveform, ramping down (identical to *saw*)  
          
      - *"sawup"* &mdash; sawtooth waveform, ramping up from -1.0 to 1.0  
          
      - *"square"* &mdash; square waveform  
          
      - *"squareX"* &mdash; square waveform like *square* but using only the
        first *X* harmonics  
          
      - *"tri"* &mdash; triangle waveform  
          
      - *"triX"* &mdash; triangle waveform like *tri* but using only the first
        *X* harmonics  
          
      - *"buzz"* &mdash; pulse waveform  
          
      - *"buzzX"* &mdash; pulse waveform like *buzz* but using only the first
        *X* harmonics


    If the second *waveform\_string* specifying method is used for the
    *wave* table type, the wavetable values will be normalized to fit
    between +1.0 and -1.0.
    
    Similar to the way that the [wave3](#wave3) table type works, the
    constructed table can be used by instruments such as
    [WAVETABLE](../instruments/WAVETABLE.html) or
    [FMINST](../instruments/FMINST.html)). In RTcmix 4.0, most of these
    have been modified to include an optional parameter that will allow
    the **maketable** waveform to be used. If this parameter is present,
    any makegen-constructed wavetables will be ignored.
    
    The constructed wavetable will usually be used in its normalized
    form, i.e. the waveform will travel between -1.0 and 1.0.
    
    See the older makegen routine [gen 10](old/gen10.html) for more
    examples of how this waveform-constructing scheme operates.

  - <span id="cheby" class="internallink">*cheby*</span>  
      
    Fill a table with a curve computed using [Chebyshev
    polynomials](http://math.fullerton.edu/mathews/n2003/ChebyshevPolyMod.html).
    These curves have the property that when used as a transfer
    function, the polynomial coefficients determine the harmonic content
    of the resulting signal for a given index value. This is very useful
    for instruments like [WAVESHAPE](../instruments/WAVESHAPE.html)),
    for it allows the used to specify the harmonics that will occur in
    the output sound. The syntax is:

    ```
    table = maketable("cheby", size, index, harmonic1[, ... harmonicN])
    ```
    
    The *harmonicN* arguments determine the relative amplitude of that
    harmonic in the output spectrum when the table-lookup index is at
    the value *index*.
    
    Although this sounds complicated, it is actually very easy to use.
    See the older makegen routine [gen 17](old/gen17.html) for more
    explanation and examples of how to use Chebyshev polynomials in
    waveshaping. *"cheby"* is functionally equivalent to this gen
    routine.

  - <span id="random" class="internallink">*random*</span>  
      
    Fill a table with random (pseudorandom) numbers. The syntax (with
    one exception) is:

    ```
    table = maketable("random", size, type, min, max[, seed])
    ```
    
    The random numbers filling the table will be between the *min* value
    and the *max* value. Both of these are required arguments. The table
    can be filled with random numbers using different random-number
    distributions, as specified by the *type*. The different *types* are
    (either the string specifiers or the numeric specifiers may be
    used):
    
    *0 / "even" / "linear"* &mdash; randomly select numbers between *min* and
    *max*.  
      
    
    *1 / "low"* &mdash; randomly select numbers between *min* and *max*, but
    with a higher probability of choosing numbers nearer the *min*
    value.  
      
    
    *2 / "high"* &mdash; randomly select numbers between *min* and *max*, but
    with a higher probability of choosing numbers nearer the *max*
    value.  
      
    
    *3 / "triangle"* &mdash; randomly select numbers between *min* and *max*,
    but with the probability of choosing a value determined by a
    triangular curve with the apex at the midpoint between the *min* and
    *max* values. In other words, numbers near either *min* or *max*
    will have a very low probability of being in the table, but numbers
    half-way between will have a high probability of being chosen.  
      
    
    *4 / "gaussian"* &mdash; randomly select numbers between *min* and *max*,
    but with the probability of choosing a value determined using a
    [Gaussian](http://mathworld.wolfram.com/NormalDistribution.html)
    ('normal'; 'bell curve') probability distribution with the apex at
    the midpoint between the *min* and *max* values. Similar to how the
    *"triangle"* specifier operates.  
      
    
    *5 / "cauchy"* &mdash; randomly select numbers between *min* and *max*, but
    with the probability of choosing a value determined using a
    [Cauchy](http://www.itl.nist.gov/div898/handbook/eda/section3/eda3663.htm)
    function with the apex at the midpoint between the *min* and *max*
    values. Similar to how the *"triangle"* and *"gaussian"* specifiers
    operate.  
      
    
    *6 / "prob"* &mdash; Mara Helmuth's configurable probability distribution.
    This has a slightly different syntax:

    ```
    table = maketable("random", size, "prob", min, max, mid, tight[, seed])
    ```
      
    For all of the above, if the optional *seed* argument is 0, then the
    seed for the psuedorandom number algorithm comes from the
    microsecond system clock, otherwise the value of *seed* is used as
    the seed. Different *seed* values will generate different
    sequences of random numbers. If no *seed* argument is present, the
    seed used is 1.
    
    Note that if you don't give the *"nonorm"* optional specifier
    argument after *"random"*, then the table will be normalized, thus
    disregarding your *min* and *max* values.
    
    This table type is similar in function to the older makegen routine
    [gen 20](old/gen20.html). The original version of *gen 20* was
    written by Luke Dubois; later additions and this adaptation by John
    Gibson. Some distribution equations were adapted from Dodge and
    Jerse, *<u>Computer Music</u>.*

  - <span id="window" class="internallink">*window*</span>  
      
    Fill a table using a
    [window](http://en.wikipedia.org/wiki/Window_function/) function
    curve. The syntax is:

```
    table = maketable("window", size, type)
```
    
    Window functions are used for many signal-processing operations.
    Using this **maketable**, two common window types can be
    constructed. The *type* specifier determines the kind of window
    function to use:
    
      - *1 / "hanning"* &mdash; this will build a
        [hanning](http://www.mathworks.com/access/helpdesk/help/toolbox/signal/hann.html)
        window function in the table.  
          
      - *2 / "hamming"* &mdash; this will build a
        [hamming](http://www.mathworks.com/access/helpdesk/help/toolbox/signal/hamming.html)
        window function in the table.
    
    Either the numeric specifier or the string specifier may be used.
    This table type is similar in function to the older makegen routine
    [gen 25](old/gen25.html).

-----

### Examples

```cpp
ampenv = maketable("line", 1000, 0,0, 1,1
```

The *ampenv* table-handle variable will reference a table containing a
straight line from 0 to 1; the table will have 1000 elements.

```cpp
wave = maketable("wave", "nonorm", 2000, 0.5)
```

The *wave* table-handle variable will reference a table containing a
sine wave at half amplitude, ranging from -0.5 to 0.5. The *"nonorm"*
optional specifier prevents the table from being normalized (scaled to
fall between -1 and 1). The table will have 2000 elements.

The following score shows how the table-handle can be used
arithmetically to operate upon other parameters in an instrument (in
this case the [WAVETABLE](../instruments/WAVETABLE.html) instrument):

```cpp
env = maketable("line", 1000, 0,0, 1,1, 3,1, 4,0)
penv = maketable("line", 1000, 0,1, 1,1, 2,2, 3,2, 8,.15)
vib = maketable("wave3", "nonorm", 1000, 4.0*10, 4, 0)
pan = maketable("line", 100, 0,0, 1,1, 2,0.5)
wavt = maketable("wave", 4000, 1, 0.5, 0.3, 0.2, 0.1, 0.1)

WAVETABLE(0, 4.0, 20000 * env, (440.0 * penv) + vib, pan, wavt)
```

-----

### NOTES

The table-handle variables can be re-used during score parsing, i.e.

```cpp
for (st = 0; st < 10; st = st+1) {
   amp = maketable("curve", 1000, 0, 0, -2.0,  2.5, 1, 1.4, 3.5, 0)
   wave = maketable("wave", 1000, random(), random(), random())
   WAVETABLE(st, 3.5, 10000 * amp, irand(100.0, 1000.0), random(), wave)
}
```

Be aware, though that memory for these tables is allocated for each use.
This is a potential small memory leak for algorthmic processes that
define a large number of new tables over a long period of time.

-----

### See Also

[makeconnection](makeconnection.html), [makemonitor](makemonitor.html),
[modtable](modtable.html), [makefilter](makefilter.html),
[makeconverter](makeconverter.html), [tablelen](tablelen.html),
[copytable](copytable.html), [samptable](samptable.html),
[dumptable](dumptable.html), [plottable](plottable.html),
[mul](mul.html), [div](div.html), [sub](sub.html), [add](add.html)
