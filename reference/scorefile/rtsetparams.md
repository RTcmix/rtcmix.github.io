---
title: rtsetparams()
layout: ref
---

## rtsetparams

Set sampling rate, output channels, etc.

-----

### Synopsis

**rtsetparams**(*sampling\_rate*, *num\_channels* \[, *buffer\_size* \])

-----

### Description

Set the sampling rate, number of output channels, and (optionally) the
buffer size for an RTcmix session.

You must call **rtsetparams** before calls to [rtinput](rtinput.html),
[rtoutput](rtoutput.html), or instruments. You can only call
**rtsetparams** once in a script.

-----

### Arguments

  - <span id="item_sampling_rate">*sampling\_rate*</span>  
      
    The sampling rate for all synthesis and processing. Any input sound
    files must have this rate, and the output sound file will have this
    rate. Similarly, the input and output devices (audio hardware) will
    have this rate.

  - <span id="item_num_channels">*num\_channels*</span>  
      
    The number of output channels. This affects any file opened for
    writing by [rtoutput](rtoutput.html), as well as the audio output
    device.

  - <span id="item_buffer_size">*buffer\_size*</span>  
      
    The number of sample frames in each buffer. RTcmix instruments
    process sound in buffer-sized chunks. The buffer size determines the
    latency for real-time applications, such as sending sound from a
    microphone into a reverb instrument. For situations like that,
    you'll want to reduce the *buffer\_size* to minimize the delay
    between the input signal and its processed output. You can have a
    *buffer\_size* as small as 64 or even 32, depending on the audio
    driver and machine load.
    
    The default *buffer\_size* is 4096 sample frames. At a sampling rate
    of 44100 kHz, this gives a latency of nearly 0.1 seconds, which is
    unsuitable for real-time work.

-----

### NOTES

To have an audio device that handles input and output at the same, you
must turn on the *record* option before calling **rtsetparams**. Do this
with [set\_option](set_option.html#record) (by saying
set\_option("record = 1")).

If you want to write a sound file that has more channels than your audio
device permits, turn off the audio device with
[set\_option](set_option.html#audio) before calling **rtsetparams** (by
saying set\_option("audio = off")).

-----

### Examples

```cpp
   rtsetparams(44100, 2)
```

sets up the session for 44100 sampling rate and two output channels.

```cpp
   rtsetparams(44100, 4, 128)
```

sets up the session for 44100 sampling rate, four output channels, and a
buffer size of 128 sample frames.

```cpp
   set_option("record=TRUE")
   rtsetparams(44100, 2, 64)
   rtinput("AUDIO")
```

sets up the session for real-time processing of a signal reaching RTcmix
from the audio device. You must turn on full duplex operation to tell
the audio device to handle input and output simultaneously.

-----

### See Also

[rtinput](rtinput.html), [rtoutput](rtoutput.html),
[set\_option](set_option.html)
