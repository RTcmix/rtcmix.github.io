---
title: load()
layout: ref
---

## load

Load an RTcmix instrument for use.

-----

### Synopsis

**load**(*"INSTRUMENT"*)  
**load**(*"dsoPATHNAME"*)

-----

### Description

**load** is an essential command for all RTcmix scorefiles. **load** is
used to load in particular instrument executable-library files (DSOs)
for the RTcmix instrument(s) that will be used during the run of the
scorefile. One or more **load** commands usually follows the
[rtsetparams](rtsetparams.html) setup command.

The syntax for **load** is easy &mdash; it will either take a string with
the name of one of the distributued RTcmix instruments stored in the
RTcmix-searched DSO library (usually "RTcmix/shlib"), or it can take an
absolute or relative pathname string to a "libINSTRUMENT.so" instrument DSO
directly (the second method is how a user-developed library would be loaded
into RTcmix for use).

Here's an example from the original documentation by Doug Scott for the
**load** scorefile command:  

-----

To run [FMINST](../instruments/FMINST.html) from *CMIX*, your score
would look like this:

```
   rtsetparams(44100, 1);    /* set output params */
   rtinput("/snd/myfile.snd");

   load("FMINST");           /* this loads the FMINST DSO */

   FMINST(....);             /* use FMINST */
```

Then run the command:

```
   CMIX < scorefile
```

If you have created a custom instrument DSO following the TEMPLATE
example, you will end up with a file called libMYINSTRUMENT.so, where
"MYINSTRUMENT" is whatever you named your instrument. To use it, just
put a line:

```
load("./libMYINSTRUMENT.so");
```

at the top of your score. Note that this is specified as a file name,
where the *FMINST* was not. Any pre-existing RTcmix instrument which has
a pre-installed DSO can use the first form; custom DSOs use the latter
form.

How does this work? Most UNIX-like operating systems allow object code
to be dynamically loaded, i.e., read from a file and added to a running
program. They also include a feature for accessing the functions and
other symbols which are in that dynamically loaded object. When you give
a load("FOO") command, RTcmix looks in its default DSO location for a
file called "libFOO.so". If it doesn't find it, it returns an error
message. If it does find it, it loads it in. It then looks in that
object to see if it has the magic "profile" and/or "rtprofile"
functions. It it does, it calls them -- and this makes all the RTcmix
functions in that DSO available to CMIX\!

The only instruments that does not require an explicit **load** command
in the scorefile is [MIX](../instruments/MIX.html) because it is the
'foundation' instrument for RTcmix.

Be aware that if you load the same DSO more than once in a scorefile, it
will really get confused about the symbol-table entries (i.e. don't do
this\!).

-----

### Arguments

  - *"INSTRUMENT"* or *"dsoPATHNAME"*  
    The string *"INSTRUMENT"* is used when referencing one of the
    distributed RTcmix instruments for loading. The 'DSO's
    (dynamically-shared objects) for these generally reside in the
    RTcmix/shlib directory, and are placed there by the "make install"
    command during the RTcmix build-and-install process. The string
    *"dsoPATHNAME"* can be an absolute or relative pathname to an
    instrument DSO. These are usually named "libINSTRUMENT.so". If it is
    a relative pathname, it will be relative to the directory in which
    the CMIX command was invoked. 
    
    *NOTE: for many embedded applications
    such as [rtcmix\~](../../rtcmix_/index.html) and
    [iRTcmix](../../irtcmix/index.html), the instruments are
    'compiled-in' and do not have to be loaded.*

-----

### Examples

```cpp
   load("WAVETABLE")
   load("/usr/local/src/myinstruments/TESTINST/libTESTINST.so")
```

-----

### See Also

[rtsetparams](rtsetparams.html), [set\_option
(auto\_load)](set_option.html#auto_load), [set\_option
(dsopath)](set_option.html#dsopath)
