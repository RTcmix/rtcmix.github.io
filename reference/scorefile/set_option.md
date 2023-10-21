---
title: set\_option()
layout: ref
---

## set\_option

Turn on and off various RTcmix options.

-----

### Synopsis

**set\_option**(*option\_name* \[, *option\_name*, ... \])

Parameters inside the \[brackets\] are optional.

-----

### Description

Use **set\_option** to control the behavior of RTcmix: whether to play
audio, to report clipping and peak amplitude, to overwrite existing
sound files, and so on.

The **set\_option** command generally precedes the
[rtsetparams](rtsetparams.html) command because much of what it does
effects how [rtsetparams](rtsetparams.html) sets up the RTcmix process.

-----

### Arguments

  - <span id="item_option_name">*option\_name*</span>  
      
    An *option\_name* is a double-quoted string of the form "option =
    val", where *option* is a valid RTcmix option for controlling global
    aspects of RTcmix execution, and *val* is a boolean parameter
    determining whether to turn the option on or off, or in certain
    cases a parameter string used by internally by RTcmix (hardware
    device names, etc.). The boolean values for *val* accepted by the
    RTcmix parser are "yes", "true", "on", and "1" for turning an option
    on, and "no", "false", "off", and "0" for turning an option off. Case
    does not matter in the *option\_name* string, and spaces are
    ignored. For example:
    
    ```cpp
       set_option("audio = on")
       set_option("AUDIO=yes")
       set_option("AUDIO= TRUE")
       set_option("audio=1")
    ```
    
    are all equivalent.
    
    A call to **set\_option** can contain more than one *option\_name*,
    separated by commas. There can be more than one call to
    **set\_option** in a script. If two calls to **set\_option** in a
    script conflict, the last one will take precedence.
    
    Some older RTcmix scores will use a **set\_option** system with
    single option strings. These generally took the form "option\_on" or
    "option\_off", depending on whether you wanted to turn the option on
    or off. Again, the case used for the string is not significant.
    These older options are still valid; they are noted in the
    documentation below.
    
    The current options are:
    
      - <span id="audio">*audio = \[0|1\]*</span>  
          
        controls whether RTcmix plays audio while executing the
        scorefile. Turning audio off will cause RTcmix to execute faster
        than real-time if a soundfile is being written and the scorefile
        doesn't place a significant load on the processor or disk. For
        instruments taking real-time input, however, turning the audio
        off will defeat the input into the instrument. The older-style
        **set\_option** used the parameters *"audio\_on"* and
        *"audio\_off"* to control this option.
        
        The default is *"audio = on"*.
    
      - <span id="play">*play = \[0|1\]*</span>  
          
        turns off the audio output during scorefile execution. This
        option also interacts with the **audio** option above. The
        older-style **set\_option** used the parameters *"play\_on"* and
        *"play\_off"* to control this option.
        
        The default is *"play = on"*.
    
      - <span id="record">*record = \[0|1\]*</span>  
          
        turns off audio input during scorefile execution. This option
        also interacts with the **audio** option above. It is also
        affected by the older [full\_duplex\_on,
        full\_duplex\_off](#full_duplex) option. When *record* is off
        (or *full\_duplex\_off* is in effect), then instruments will not
        receive any real-time input. In other words, you need to set
        *"record = true"* (or the equivalent "on", "1", etc.) if you
        want to process sound from a mic or line real-time input. Note
        that this option should be set prior to a call to the
        [rtsetparams](rtsetparams.html) scorefile command. The
        older-style **set\_option** used the parameters *"record\_on"*
        and *"record\_off"* to change this option.
        
        The default is *"record = off"*, except in the
        [rtcmix\~](../../rtcmix_/index.html) object; *"record = on"* is
        always enabled for the object.
    
      - <span id="require_sample_rate">*require\_sample\_rate = \[0|1\]*</span>  
          
        determines whether RTcmix will fail if it cannot set the device
        sampling rate to what the score requests, or if it should allow
        whatever it is able to set it to.  Note that any input sounds will
        be played at the wrong pitch if the sample rate does not match
        that of the sounds.
  
      - <span id="clobber">*clobber = \[0|1\]*</span>  
          
        controls whether RTcmix overwrites existing sound files without
        asking. A "true" ("1", "on", etc.) value for *clobber* means
        that overwriting is enabled. The older-style **set\_option**
        used the parameters *"clobber\_on"* and *"clobber\_off"* to
        control this option.
        
        The default is *"clobber = off"*.
    
      - <span id="print">*print = \[0|1\]*</span>  
          
        determines whether or not RTcmix will print information to the
        standard output and standard error output (i.e. the terminal
        window) related to scorefile execution -- note parsing and
        scheduling information, instrument messages, etc. See also the
        [print\_on](print_on.html) and [print\_off](print_off.html)
        scorefile commands.
        
        The default is *"print = on"*.
    
      - <span id="print">*print\_list\_limit = n\_items*</span>  
          
        determines how many items will be displayed when RTcmix prints
        the contents of a Minc list during [print](print.html),
        [printf](printf.html), and Minc command parsing.  Sometimes
        complex scores can use *very long* lists, and limiting what
        is displayed can keep the text information readable.
        
        The default is *"print\_list\_limit = 16"*.

      - <span id="report_clipping">*report\_clipping = \[0|1\]*</span>  
          
        controls whether RTcmix reports samples that exceed the range
        provided by signed 16-bit integers. The older-style
        **set\_option** used the parameters *"report\_clipping\_on"* and
        *"report\_clipping\_off"* to set this option.
        
        The default is *"report\_clipping = on"*.
    
      - <span id="check_peaks">*check\_peaks = \[0|1\]*</span>  
          
        controls whether RTcmix checks, reports, and stores peak
        amplitude stats in soundfile headers. The older-style
        **set\_option** used the parameters *"check\_peaks\_on"* and
        *"check\_peaks\_off"* to alter this option.
        
        The default is *"check\_peaks = on"*.
    
      - <span id="exit_on_error">*exit\_on\_error = \[0|1\]*</span>  
          
        determines whether or not RTcmix exits the process when a
        'fatal' error is encountered. Generally this is the desired
        behavior, but if RTcmix is used [embedded within another
        application](../../tutorials/embed.html), then a full exit may
        not be appropriate.
        
        The default is *"exit\_on\_error = on"*, except in the
        [rtcmix\~](../../rtcmix_/index.html) object; *"exit\_on\_error =
        off"* is set because shutting down the entire max/msp patch or
        application for an RTcmix error probably isn't a good idea.
    
      - <span id="bail_on_parser_warning">*bail\_on\_parser\_warning = \[0|1\]*</span>  
          
        determines whether or not RTcmix exits the process when the
        parser generates a 'warning' message. Generally this is not necessary,
        but if your score does not behave as you expect, sometimes exiting
        for minor mistakes is the best way to find the problem.
        
        The default is *"bail\_on\_parser\_warning = 0"*.

      - <span id="auto_load">*auto\_load = \[0|1\]*</span>  
          
        sets the ability of RTcmix to automatically load Instruments if
        an Instrument name is parsed. If this option is set to "true",
        then an explicit [load](load.html) command is not required for
        each Instrument used in the score. The default directory for
        Instrument shared libraries is used (usually RTcmix/shlib), or a
        different directory can be set using the [dsopath](#dsopath)
        option.
        
        The default is *"auto\_load = off"*.
    
      - <span id="fast_update">*fast\_update = \[0|1\]*</span>  
          
        allows RTcmix to use a direct-access of static table functions
        for certain instruments, thus increasing the efficiency of these
        instruments. Using this option will disable the
        [PField-updating](../instruments/pfield-enabled.html) scheme,
        however. Tables for waveforms and envelopes need to be
        pre-created using *fast\_update*.
        
        The current list of instruments with this capability includes:
        [WAVETABLE](../instruments/WAVETABLE.html),
        [FMINST](../instruments/FMINST.html),
        [MIX](../instruments/MIX.html), [IIR](../instruments/IIR.html),
        [STEREO](../instruments/STEREO.html) and
        [TRANS](../instruments/TRANS.html).
        
        The default is *"fast\_update = off"*.
    
      - <span id="full_duplex">*full\_duplex\_on,
        full\_duplex\_off*</span>  
          
        controls whether RTcmix opens the audio device for simultaneous
        input and output. This option has largely been superceded by the
        [record](#record) option. Note that this option should be set
        prior to a call to the [rtsetparams](rtsetparams.html) scorefile
        command, like the *record* option.
        
        The default is *"full\_duplex = off"*, except in the
        [rtcmix\~](../../rtcmix_/index.html) object; *"full\_duplex =
        on"* is always enabled for the object.
    
      - <span id="buffer_frames">*buffer\_frames = nsamps*</span>  
          
        This option sets the default number of samples (*nsamps*) in
        each buffer used by RTcmix. Although this seems a rather
        esoteric paramater, it can have a noticeable effect on RTcmix
        performance, especially in an interactive application. This
        option should be set prior to a call to the
        [rtsetparams](rtsetparams.html) scorefile command, and in fact
        the third (optional) paramater for
        [rtsetparams](rtsetparams.html) will override this default
        setting.
        
        The default is *"buffer\_frames = 4096"*, except in the
        [rtcmix\~](../../rtcmix_/index.html) object. The value for
        *nsamps* is determined by max/msp.
    
      - <span id="buffer_count">*buffer\_count = nbufs*</span>  
          
        This sets the number of buffers (*nbufs*) used by the audio
        conversion device. It is, in fact, a rather esoteric parameter,
        although it can have an effect on RTcmix perfoprmance.
        
        The default is *"buffer\_count = 2"*.
    
      - <span id="device">*device = audio\_device\_name*</span>  
          
        *indevice = audio\_device\_name*  
          
        *outdevice = audio\_device\_name*  
          
        This option specifies the audio device to use for input and
        output. The *audio\_device\_name* is a string referring to a
        particular device. For a discussion of device names used, see
        the [ALSA
        Project](http://www.alsa-project.org/alsa-doc/doc-php/asoundrc.php?module=Generic)
        device name page. See also the discussion of input sources for
        the [rtinput](rtinput.html#item_input_source) command.
        
        The *device* option is useful for specifying multichannel output
        devices, taking advantage of the multichannel capability of
        RTcmix. Different devices can be set for input or output using
        the *indevice* or *outdevice* options.
        
        The default is to use the internal computer hardware device.
    
      - <span id="midi_device">*midi\_indevice =
        midi\_device\_name*</span>  
          
        *midi\_outdevice = midi\_device\_name*  
          
        These set the MIDI devices to use for MIDI input
        (*midi\_indevice*) and output (*midi\_outdevice*). As with the
        audio device, see the [ALSA
        Project](http://www.alsa-project.org/alsa-doc/doc-php/asoundrc.php?module=Generic)
        device name page for a discussion of how the MIDI devices used
        are named.
        
        The default is to use the internal computer hardware MIDI device
        (if it exists).
    
      - <span id="dsopath">*dsopath = directory\_path*</span>  
          
        sets up RTcmix to look for Instrument shared libraries in the
        directory specified by *directory\_path*. This can be an
        absolute or relative pathname to a directory. This directory is
        searched by the [load](load.html) operation.
        
        The default is to use the default RTcmix/shlib directory.
    
      - <span id="rcname">*rcname = filename*</span>  
          
        This causes RTcmix to load the file *filename* containing
        settings for the various options outlined above. See the
        [note](#rtcmixrcnote) below...
        
        The default setting is to use the *.rtcmixrc* file in the user's
        home directory.
    
      - <span id="homedir">*homedir = pathname*</span>  
          
        This will set the internal RTcmix 'homeDir' variable to an
        arbitrary directory.
        
        The default setting is to use the user's home directory. A whole
        lotta usin' goin' on.

  

-----

<span id="rtcmixrcnote"></span>

*NOTE: All of these options may also be specified in a file* .rtcmixrc
*created in your home directory. Check the documentation for the*
[setup\_rtcmixrc](../utilities/setup_rtcmixrc.html) *utility command.*

-----

  

-----

### Examples

```cpp
   set_option("audio = off", "clobber=1")
   rtoutput("myfile.aif")
```

turns off audio playback and enables overwriting of existing files. This
is the standard way to make a script that writes a sound file.

```cpp
   set_option("audio=false")
   set_option("clobber = YES")
   rtoutput("myfile.aif")
```

this does exactly the same thing.

```cpp
   set_option("record = on")
   rtsetparams(44100, 2, 128)
   rtinput("AUDIO")
```

sets up RTcmix to process audio received from the audio device in real
time.

-----

### See Also

[load](load.html), [print\_off](print_off.html),
[print\_on](print_on.html), [rtsetparams](rtsetparams.html),
[rtinput](rtinput.html), [rtoutput](rtoutput.html)
