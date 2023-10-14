---
title: rtinput()
layout: ref
---

## rtinput

Open a sound file or audio device for reading.

-----

### Synopsis

**rtinput**(*"input\_source"*)

-----

### Description

Call **rtinput** to open a sound file, or an audio device, for
subsequent reading by real-time instruments.

("Audio device" just refers to the hardware that handles sound I/O,
such as a sound card on a PC or the built-in hardware on a Mac.
Different device types may be specified using the
[set\_option](set_option.html#device) scorefile command.)

After **rtinput** opens a sound file, it prints information about the
file, such as the header type and sampling rate (unless the
[print\_off](print_off.html) scorefile command has been issued).

-----

### Arguments

  - <span id="item_input_source">*"input\_source"*</span>  
      
    A text string with the name of a sound file, in double-quotes,
    having a header format that RTcmix understands. (These include, but
    are not limited to, AIFF, AIFC, WAVE, NeXT, and IRCAM.) Only files
    with data formats of 32-bit floating point or 16-bit or 24-bit
    integer are readable. NOTE: MP3 and other compressed formats are not
    supported at present.
    
    If *input\_source* is "AUDIO", then input comes from the audio
    device. This lets you send input to RTcmix from a microphone or
    line-level source. The mic/line audio device is selected using the
    [set\_option](set_option.html#device) scorefile command. For linux
    users, there is a utility program called "alsaprobe" in
    RTcmix/test/alsa that can list the available devices. A similar
    program exists for macOS users ("coreaudioprobe") in
    RTcmix/test/coreaudioprobe. For 'full duplex' (i.e. input and output
    simultaneously) use of RTcmix, macOS users need to create an
    "aggregate device" using the AudioMIDISetup.app (in
    /Applications/Utilities) with both in and out capabilities. As an
    example, this "aggregate device" is selected using
    [set\_option](set_option.html#device) as followe:
    
    Note that the [set\_option](set_option.html#device) command has to
    precede the [rtsetparams](rtsetparams.html) command in the
    scorefile.

-----

### Examples

``` 
   rtinput("myfile.aif")
```

Opens "myfile.aif,'' an AIFF file in the current directory, for reading
by any instruments that follow this line in the script.

``` 
   rtinput("/home/bubba/snd/trouble.wav")
```

Opens "trouble.wav'' using a full path name.

``` 
   rtinput("AUDIO)"
```

Opens the audio device for reading.

-----

### See Also

[bus\_config](bus_config.html), [rtsetparams](rtsetparams.html),
[rtoutput](rtoutput.html), [set\_option](set_option.html),
[DUR](DUR.html), [PEAK](PEAK.html), [SR](SR.html),
[filechans](filechans.html), [filedur](filedur.html),
[filepeak](filepeak.html), [filesr](filesr.html)
