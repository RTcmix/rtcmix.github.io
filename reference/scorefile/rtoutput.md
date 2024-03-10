---
title: rtoutput()
layout: ref
---

## rtoutput

Open a new sound file for writing.

-----

### Synopsis

**rtoutput**(*"file\_name"* \[, *"header\_type"* \] \[, *"data\_format"*
\])

Parameters inside the \[brackets\] are optional.

-----

### Description

Call **rtoutput** to open a new sound file for subsequent writing by
real-time instruments.

After **rtoutput** creates a sound file, it prints information about the
file, such as the header type and sampling rate (unless the
[print\_off](print_off.html) scorefile command has been issued).

-----

### Arguments

  - <span id="item_file_name">*"file\_name"*</span>  
      
    A text string with the name of a sound file, in double-quotes. If
    the file already exists, the script will terminate with an error,
    unless you've turned on file clobbering with
    [set\_option](set_option.html#clobber) (by saying
    set\_option("clobber = on")). If you name the file with a recognized
    suffix, that suffix will determine the header type, unless
    overridden by a *"header\_type"* string. Recognized suffixes:
    ".wav'', ".aif'', ".aiff'', ".aifc'', ".snd'' (NeXT/Sun), ".au''
    (NeXT/Sun), ".sf'' (IRCAM), and ".raw''.

  - <span id="item_header_type">*"header\_type"*</span>  
      
    The type of header to use for the sound file, as a double-quoted
    string. Note that if you name the file with a recognized suffix (see
    above), you don't need to specify a header type in this way.

    AIFF is the default if no header type is given.

  - <span id="item_data_format">*"data\_format"*</span>  

    The type of data format to use for the sound file, as a double-quoted
    string.

NOTE: The sampling rate and number of channels are specified in a call
to [rtsetparams](rtsetparams.html) at the beginning of the script.

"short" is the default if no data format is given.

-----

### NOTES

If you don't want RTcmix to play while you're writing a file, use
[set\_option](set_option.html#audio) to turn off playing before you
invoke [rtsetparams](rtsetparams.html), by saying set\_option("audio = off").

The case of the *header\_type* and *data\_format* arguments is not
significant, nor is their order.

All formats are big-endian, except for "wav," which is always
little-endian, and "raw," which has host byte order.

If you ask for "aiff" and "float" (or "normfloat"), you'll get
"aifc" format instead, because AIFF doesn't support floating-point
files.

Although most soundfile programs now deal with nearly all soundfile types, few
of them expect floating-point files to use the unnormalized range of values
that RTcmix uses by default (-32768.0&ndash;32767.0 and beyond). When writing
floating-point files that you want to open in other programs, use the
"normfloat" format, where the values are normalized to fit within the
-1.0&ndash;1.0.

If you want to use them in
[MiXViews](https://music.columbia.edu/~doug/MixViews/MiXViews.html)
editor, choose the "next" header type. Many programs don't read AIFC
files, maybe because they assume these are always compressed.

-----

### Examples

```cpp
   rtsetparams(22050, 2)
   rtoutput("mysound")
```

writes a stereo, 16-bit linear AIFF file with 22050 sampling rate.

```cpp
   rtsetparams(44100, 1)
   set_option("audio = off", "clobber = on")
   rtoutput("myothersound", "wav", "float")
```

writes a mono, 32-bit floating-point WAV file with 44100 sampling rate.
RTcmix will write over any existing file with the same name, and will
not play audio while writing.

-----

### See Also

[bus\_config](bus_config.html), [rtsetparams](rtsetparams.html),
[rtoutput](rtoutput.html), [set\_option](set_option.html)
