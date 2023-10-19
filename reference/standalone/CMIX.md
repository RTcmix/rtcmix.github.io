---
title: CMIX()
layout: ref
---

### CMIX/PCMIX/PYCMIX

*INTERFACE -- the main commands used to invoke RTcmix*

These commands are used to start and run RTcmix from the command-line;
i.e. typed into a terminal. **CMIX** is the default command, it employs
the included [Minc](../scorefile/Minc.html) parsing language to
interpret scorefiles. **CMIX** can also be run interactively with
scorefile commands typed directly into a terminal window running the
**CMIX** command, although this requires very careful typing skills.

The **PCMIX** command is available if RTcmix is configured and compiled
with the *--with-perl* configuration flag (see the [installation
guide](../../rtcmix/index.html) for information about how to do this) --
it uses the Perl language as the scorefile parser.

Similarly, the Python language may be used if the **PYCMIX** command was
created (done with the *--with-python* configure flag).

All three commands can be run with various options:

``` 
   Usage:  CMIX [options] [arguments] < minc_script.sco

   or, to use Perl instead of Minc:
        PCMIX [options] [arguments] < perl_script.pl
   or:
        PCMIX [options] perl_script.pl [arguments]

   or, to use Python:
        PYCMIX [options] [arguments] < python_script.py

        options:
           -i       run in interactive mode
           -n       no init script (interactive mode only)
           -o NUM   socket offset (interactive mode only)
           -c       enable continuous control (rtupdates)
          NOTE: -s, -d, and -e are not yet implemented
           -s NUM   start time (seconds)
           -d NUM   duration (seconds)
           -e NUM   end time (seconds)
           -f NAME  read score from NAME instead of stdin
                      (Minc and Python only)
           --debug  enter parser debugger (Perl only)
           -q       quiet -- suppress print to screen
           -Q       really quiet -- not even clipping or peak stats
           -h       this help blurb
        Other options, and arguments, passed on to parser.
```
