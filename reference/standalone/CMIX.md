---
title: CMIX()
layout: ref
---

## CMIX/PCMIX/PYCMIX

*INTERFACE -- the main commands used to invoke RTcmix*

These commands are used to start and run RTcmix from the command-line,
i.e. typed into a terminal. **CMIX** is the default command; it employs
the included [Minc](../scorefile/Minc.html) parsing language to
interpret scorefiles. **CMIX** can also be run interactively with
scorefile commands typed directly into a terminal window running the
**CMIX** command, although this requires very careful typing skills.

The **PCMIX** command is available if RTcmix is configured and compiled
with the *--with-perl* configuration flag (see the [installation
guide](../../standalone/index.html) for information about how to do this) --
it uses the Perl language as the scorefile parser.

Similarly, the Python language may be used if the **PYCMIX** command was
created (done with the *--with-python* configure flag).

All three commands can be run with various options:

### Usage:
- To use Minc parser:
	- **CMIX** [options] [arguments] < minc_script.sco **or**

	- **CMIX** [options] [arguments] -f minc_script.sco

- To use Perl instead of Minc:
	- **PCMIX** [options] [arguments] < perl_script.pl **or**

	- **PCMIX** [options] perl_script.pl [arguments]

- To use Python:
	- **PYCMIX** [options] [arguments] < python_script.py


### Universal Options (CMIX, PCMIX, and PYCMIX)
-       -i  
run in interactive mode
-       -n  
no init script (interactive mode only)
-       -o  
interactive mode with [Open Sound Control (OSC)](https://opensoundcontrol.stanford.edu) (if compiled in)
-       -S NUM  
socket offset (interactive legacy socket mode only)
-       -D DEV  
use audio device DEV for playback
-       -r IPR  
use remote IP address IPR for NetAudio (if compiled in)
-       -k NUM  
use socket number NUM for NetAudio (if compiled in)
-       -s NUM  
skip (set start offset to) NUM seconds. Time offset before beginning playback (see [rtoffset](../scorefile/rtoffset.html))
-       -f NAME  
read score from NAME instead of stdin (Minc and Python only)
-       -v NUM  
set verbosity (print level) to NUM.  Range is currently 0-5
-       -q  
quiet -- suppress print to screen.  Equivalent to -v 0
-       -Q  
really quiet -- not even clipping or peak stats
-       -h  
this help message
    
### CMIX-specific Options
-       --VARNAME=VALUE, --VARNAME2=VALUE2, ...  
for any argument of this type, create a score variable named $VARNAME, $VARNAME2, etc., with the associated value.  See [Command-line named arguments](../scorefile/Minc.html#command-line-named-args)
    
### PCMIX-specific Options
-           --debug  
enter parser debugger
    
### Any other options or arguments will be passed on to the parser.
