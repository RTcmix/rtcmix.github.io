---
title: hist()
layout: ref
---

**hist** is an older utility that allows you to quickly plot a scan of
the amplitude of a soundfile, or an FFT representation of part of a
soundfile. Typing "hist" with no arguments gives the following usage
statement:

    Usage:  hist [options] filename                                     
            options:  -p   (no star plots)                              
                      -n   (max number of stars per line - default: 40) 
    
    Run-time commands: 
            time_increment     start_time      end_time       (seconds)       
       OR:  -nsamps_increment  -start_samp     -nsamps_dur    (frame numbers) 
                                                                              
          - For RMS amplitudes, make either start_time OR end_time negative.     
          - Shows all chans by default. To show a range of chans, append this:   
               first_channel      last_channel    (starting from 0)              
          - Takes overall peak from header. To override, give after chan range.  
          - To plot an FFT...                                                    
               size_of_fft        start_time      channel                        
                    ^------ (power of 2 btw. 32 and 2048) 

This is from the original documentation for the command:

-----

### Syntax

``` 
     hist [-p -n nstars] filename
```

-----

### Description

Invoking hist for a specified soundfile will throw you into an
interactive program where you will be prompted for specifications. The
syntax of these specifications is:

time increments, start, end, \[channel 1st, channel last, peak\]

Time increments specifies the amount of time to scan for peak amplitude
at a time, and start and end are, of course, the time to start and end
the scan. A plot will be drawn at your terminal showing the relative
amplitudes in stars. If time increments is a negative number it
represents the number of samples to gulp at a time, and in addition, the
plot will show positive and negative values and will be drawn about a
central vertical axis on your screen. If the start and end arguments are
negative the first will indicate the starting sample number and the
second will indicate the number of samples to scan altoghether.

If the time increments argument is positive, and either the start or end
arguments are negative, this is an indication to plot by rms amplitude
rather than by peak amplitude for the given segments.

For positive time increments the plot drawn represents the 'or' of the
amplitudes of all the channels. If, however, you want to plot one
channel, or a selected group of channels, the 4th and 5th arguments can
optionally specify the range of channels. If only a 4th argument is
specified the 5th argument defaults to the 4th argument.

Normally, the plot is scaled by the peak amplitude found in the header
of the soundfile. If, however, you would like to respecify that for a
given segment, use the optional 6th argument to specify a new peak. This
is useful for a section of a soundfile in which the amplitudes are very
soft and the plot would normally be quantized to 0.

The -p flag on the command line will force a session in which no
plotting is done at all. The -n flag, followed by nstars indicates the
number of stars to be used in the plot of a maximum amplitude. The
default is 40, which fits nicely on a standard crt screen. If the peak
amplitude found in the header is incorrect and is smaller than the
actual peak amplitude on the file, any segment in which this peak is
found will be drawn with the maximum number of characters allowed and
the sign '\>' will be substituted for '\*'.

Signals are trapped in this program so that \<cntrl-C\> will terminate
the current plot and return you to the time specification. Another
\<cntrl-C\> will terminate the session.

If your time increment is 32, 64, 128, 256, 512, 1024 or 2048, it is a
flag to the program to perform an fft rather than an amplitude plot. The
next argument is, as before, the time to begin (in seconds, or if
negative, samples--per channel), but the third argument is now the the
channel to plot. In this case there are no further relevant arguments.

### SEE ALSO

[sfcreate](sfcreate.html)
