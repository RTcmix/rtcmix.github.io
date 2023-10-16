---
title: PFSCHED()
layout: ref
---

## PFSCHED

Dynamically schedule pfield controls.

*in RTcmix/insts/bgg*  
  

-----

##### quick syntax:

**PFSCHED**(outsk, dur, pfield\_bus, pfield\[, deleteflag\])

-----

  

``` 
   p0 = output start time (seconds)
   p1 = duration (seconds)
   p2 = pfield bus number to use
   p3 = pfield variable
   p4 = delete note flag (keep note active: 0, delete note: 1) [optional; default is 0]

   Author Brad Garton, 11/2009
```

  

-----

  
**PFSCHED** allows you to schedule pfield control information
dynamically as a note is executing.

### Usage Notes

The **PFSCHED** instrument takes advantage of the
[pfield-enabled](pfield-enabled-2.html) control capabilities of RTcmix
instruments to arbitrarily schedule pfield controls during execution of
a note. It uses an internal set of "pfield busses" (up to 1024) for the
routing of pfield information to instruments/notes. This is useful in
interactive RTcmix applications such as the
[rtcmix\~](../../rtcmix_/rtcmix_.php/index.html) Max/MSP object or the
[iRTcmix](../../irtcmix/index.html) library for iOS.

The "pfield bus" (or "pfbus") is set up using the
[makeconnection("pfbus")...](../scorefile/makeconnection.html#pfbus)
scorefile command. Once a pfield variable is created using this
makeconnection(), it can be used to send other pfield control data
through the "pfbus" associated with the makeconnection() pfield
variable.

Here's how it works in practice: Suppose you want to start a
[WAVETABLE](WAVETABLE.html) note with an arbitrary duration, but at some
point in the future you want to fade that note down (and possibly end
it).

  - First of all, create a "pfbus" (and associated pfield variable)
    using
    [makeconnection("pfbus")...](../scorefile/makeconnection.html#pfbus):
    
    ``` 
       pfield_var = makeconnection("pfbus", 1, 1.0)
    ```
    
    This will create a pfield named "pfield\_var" that will receive
    information through "pfbus" \#1 with an initial value of 1.0.  
      
      

  - Next start the [WAVETABLE](WAVETABLE.html) note with an arbitrarily
    long duration, and the pfield variable "pfield\_var" set to control
    the amplitude of the note:
    
    ``` 
       WAVETABLE(0, 7.0, 20000 * pfield_var, 8.00)
    ```
    
    Note that because "pfield\_var" has an initial value of 1.0 that the
    [WAVETABLE](WAVETABLE.html) will be producing sound at full
    volume.  
      
      

  - Then create an envelope that will fade down and assign it to a
    different pfield variable:
    
    ``` 
       delayed_envelope = maketable("line", 1000, 0,1.0,  1,0.0)
    ```
    
    This uses the
    [maketable("line",...)](../scorefile/maketable.html#line) scorefile
    command to fill a table with values going from 1.0 to 0.0. Note that
    this envelope can be created before starting the
    [WAVETABLE](WAVETABLE.html) note, or at any point while the note is
    executing.  
      
      

  - This control envelope -- using the pfield variable it assigned --
    can now be scheduled through the "pfbus" using **PFSCHED**:
    
    ``` 
       PFSCHED(2.5, 2.1, 1, delayed_envelope)
    ```
    
    This will cause the "delayed\_envelope" fade-out table to be sent to
    the executing [WAVETABLE](WAVETABLE.html) note at 2.5 seconds (p0,
    "outsk") from the time the **PFSCHED** instrument command was
    received by RTcmix. The "delayed\_envelope" will last for a duration
    of 2.1 (p1, "dur") seconds. It is sent to the
    [WAVETABLE](WAVETABLE.html) note through "pfbus" \# 1 (p2,
    "pfield\_bus"). This "pfbus" is connected to the
    [WAVETABLE](WAVETABLE.html) through the pfield variable
    "pfield\_var", setup earlier with the
    [makeconnection("pfbus")...](../scorefile/makeconnection.html#pfbus)
    scorefile command.  
      
      

  - If the optional paramater p4 ("deleteflag") had been set to 1, then
    the [WAVETABLE](WAVETABLE.html) note would stop executing and be
    deleted when the "delayed\_envelope" was finished (the 2.1 second
    duration). If p4 had not been set to 1, then the
    [WAVETABLE](WAVETABLE.html) would continue executing (although with
    0.0 amplitude in our example here) and could receive additional
    **PFSCHED** control information in the future.
    
    NOTE: To check if a particular instrument/note is still active, the
    [note\_exists](../scorefile/note_exists.html) scorefile command.

  
Any pfield may be scheduled using **PFSCHED**, including pfield
variables associated with active pfield generators such as
[makeLFO](../scorefile/makeLFO.html) and
[makerandom](../scorefile/makerandom.html)

One feature of the
[maketable("line",...)](../scorefile/maketable.html#line) scorefile
command that is very useful in conjunction with **PFSCHED** is the use
of the "curval" token with the "dynamic" optional specifier:

``` 
   fadenv = maketable("line", "dynamic", 1000, 0,"curval", 1,0.0)
```

This will build the "line" maketable using the current value of a pfield
associated with a "pfbus". The table doesn't actually get created until
the **PFSCHED** instrument schedules the table for use. This allows it
to check the current value of the pfield associated with a "pfbus" and
use it to build the table. In other words, you can begin a fade (like
the above example) using whatever value is set for an instrument/note
when the pfield control is scheduled using **PFSCHED**

### Sample Scores

very basic:

``` 
   rtsetparams(44100, 1)
   load("PFSCHED")
   load("WAVETABLE")

   value = makeconnection("pfbus", 1, 0.0)

   WAVETABLE(0, 77.0, 20000*value, 8.00)

   delayed_envelope = maketable("line", 100, 0,0.0,  1,1.0)
   PFSCHED(2.5, 2.1, 1, delayed_envelope)
```

  
  
slightly more advanced:

``` 
   rtsetparams(44100, 1)
   load("PFSCHED")
   load("WAVETABLE")

   reset(10000)

   fadeup_envelope = maketable("line", 1000, 0,0.0,  1,1.0)
   controlpfield = makeconnection("pfbus", 1, 0.0)

   PFSCHED(4.5, 0.01, 1, fadeup_envelope)

   WAVETABLE(0, 777.0, 20000*controlpfield, 8.00)

   fadedown_envelope = maketable("line", "dynamic", 1000, 0,"curval",  1,0.0)
   //note that deleteflag is set; the note will be deleted when fadedown_envelope finishes
   PFSCHED(6.2, 1.5, 1, fadedown_envelope, 1)
```

  
  
fun stuff\!

``` 
   rtsetparams(44100, 2)
   load("PFSCHED")
   load("WAVETABLE")

   delayed_envelope = maketable("line", 1000, 0,0.0,  1,1.0)
   ampvalue1 = makeconnection("pfbus", 1, 0.0)
   ampvalue2 = makeconnection("pfbus", 2, 1.0)
   PFSCHED(0.5, 3.5, 1, delayed_envelope)

   delayed_LFO = makeLFO("sine", 5.0, 0, 0.03)
   pitchvalue = makeconnection("pfbus", 3, 0.0)
   PFSCHED(4.0, 3.5, 3, delayed_LFO)

   wave = maketable("wave", 1000, "saw9")

   WAVETABLE(0, 777.0, 20000*ampvalue1*ampvalue2, 8.00+pitchvalue, 0.5, wave)

   fade_env =  maketable("line", 1000, 0,1.0,  1,0.0)
   PFSCHED(5.2, 1.4, 2, fade_env, 1)
```

  

-----

### See Also

[makeconnection](../scorefile/makeconnection.html),
[maketable](../scorefile/maketable.html),
[makeLFO](../scorefile/makeLFO.html), [WAVETABLE](WAVETABLE.html)
