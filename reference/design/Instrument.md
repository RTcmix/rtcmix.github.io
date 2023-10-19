---
title: Instrument()
layout: ref
---

### Instrument

*INSTRUMENT design -- base class for INSTRUMENT creation*  
  
The **Instrument** object is used as the base class for deriving RTcmix
user-written INSTRUMENTS. It is never used directly, although many of
its attributes are essential to RTcmix INSTRUMENT design. Someone
writing an RTcmix instrument generally only needs to fill in two of the
Instrument methods: the *INSTRUMENT::init()* method for initializing
INSTRUMENT notes, and the *INSTRUMENT::run()* method for computing
samples. See the RTcmix instrument design
[tutorial](../../tutorials/instrumentdesign.html) for how this is done.

-----

### Class Variables

*NOTE: Although older RTcmix instruments access and modify many of these
variables directly, it is better to use the [Access Methods](#access)
described below. In fact, I'm not even sure that these variables can be
used directly, but it's probably good to know about them. So here they
are...*

  - *float* **\_start** is the starting time (in seconds) of the
    scheduled INSTRUMENT note. This is set by the
    [rtsetoutput](rtsetoutput.html) function. Use the
    **[getstart](#getstart)** method to retrieve the value of this
    variable.  
      
  - *float* **\_dur** is the duration (in seconds) of the scheduled
    INSTRUMENT note. This is set by the [rtsetoutput](rtsetoutput.html)
    function. Use the **[getdur](#getdur)** method to retrieve the value
    of this variable.  
      
  - *int* **cursamp** is the current sample being computed. The user
    should increment this variable in the sample-computing loop using
    the **[increment](#increment)** method:
    **cursamp** will range from *0* to *\_nsamps-1* during note
    execution. Use the **[currentFrame](#currentFrame)** method to
    retrieve the value of this variable.  
      
  - *int* **chunksamps** is the number of samples ("frames") to be
    computed during each RTcmix scheduler "chunk". The
    *INSTRUMENT::run()* method is called once for each scheduler "chunk"
    for every note executing at a given time. Use the
    **[framesToRun](#framesToRun)** method to retrieve the value of this
    variable. Also, the **[setchunk](#setchunk)** method can be used to
    change the value of this variable, although this may negatively
    affect the execution of RTcmix.  
      
  - *int* **endsamp** is the ending sample number (frame). This is an
    absolute number from the very start of RTcmix running (I think...).
    In other words, it is (start+dur)\*SR. Use the
    **[getendsamp](#getendsamp)** method to retrieve the value of this
    variable. The **[setendsamp](#setendsamp)** method can be used to
    change the value of this variable.  
      
  - *int* **\_nsamps** (formerly **nsamps**) is the number of samples
    ("frames") to be executed for the entire note. This is computed by
    RTcmix. In RTcmix versions prior to 4.0, it used to be set by the
    [rtsetoutput](rtsetoutput.html) function Use the
    **[nSamps](#nSamps)** method to retrieve the value of this
    variable.  
      
  - *struct InputState* **\_input** is a structure containing
    information about the input devices and/or files to RTcmix. The
    structure is defined as:
    Two members of this structure have been used in older instruments as
    Instrument class variables:  
      
      - *double* **inputsr** is the sampling rate of the input soundfile
        or input device (in samples/second).  
          
      - *int* **inputchans** is the number of input channels for the
        input soundfile or input device.
      
      
    *inputsr* and *inputchans* can no longer be accessed directly. Use
    the **[inputChannels](#inputChannels)** method to retrieve the value
    of the *inputchans* variable. The *inputsr* sampling rate is assumed
    to be the same as the value of the *SR* variable.  
      
  - *int* **outputchans** is the number of output channels for the
    output soundfile or output device. Use the
    **[outputChannels](#outputChannels)** method to retrieve the value
    of this variable.  
      
  - *int* **\_skip** is the number of samples corresponding to the
    current *resetval* set by the [reset](../scorefile/reset.html)
    scorefile command. It is accessed through the
    **[getSkip](#getSkip)** member function, and can be used in setting
    up control-rate branches during note execution. The default value is
    1000.  
      
  - *float* **SR** -- the output sampling rate (in samples/second) set
    by the [rtsetparams](../scorefile/rtsetparams.html) directive in the
    scorefile.  
      
  - *int* **NCHANS** -- the number of channels in the output (should be
    the same is *outputchans* set by the
    [rtsetparams](../scorefile/rtsetparams.html) directive in the
    scorefile.  
      
  - *int* **RTBUFSAMPS** -- the number of samples ("frames") in each
    RTcmix buffer (should be the same as *chunksamps*, I believe). This
    can be set by the [rtsetparams](../scorefile/rtsetparams.html)
    directive in the scorefile, although it is an optional parameter.
    The default value varies depending on the computing platform.  
      
      
      
    There are a bunch of other variables associated with the
    **Instrument** object, but I'll be blamed if I can recall or
    ascertain what they all do. Aaah\! I'm old\! Many of them are used
    internally by RTcmix during execution, and are of limited utility
    for an instrument designer. See the *Instrument.h* and
    *Instrument.cpp* files in the "RTcmix/src/rtcmix/" directory for a
    complete look at these.  
      

<span id="access"></span>

-----

### Access Methods

<span id="currentFrame"></span> **int Instrument::currentFrame()**  
  

returns the current sample (frame) being computed. This is the value of
the variable *cursamp*.

  
  
<span id="framesToRun"></span> **int Instrument::framesToRun()**  
  

returns the number of samples (frames) to compute in each invocation of
the **Instrument::run()** method. Usually this is the value of the
variable *chunksamps*.

  
  
<span id="nSamps"></span> **int Instrument::nSamps()**  
  

returns the total number of samples (frames) to run for the note. This
is the value of the variable *\_nsamps*.

  
  
<span id="inputChannels"></span> **int Instrument::inputChannels()**  
  

returns the number of input channels for the input device or file
(specified in the [rtinput](../scorefile/rtinput.html) scorefile
directive). This is the value of the variable *inputchans*.

  
  
<span id="outputChannels"></span> **int Instrument::outputChannels()**  
  

returns the number of output channels for the output device or file
(specified in the [rtsetparams](../scorefile/rtsetparams.html) scorefile
directive). This is the value of the variable *outputchans*.

  
  
<span id="getPFieldTable"></span> **float\*
Instrument::getPFieldTable(*int index, int \*tablelen*)**  
  

returns a pointer to the floating-point array of a table constructed by,
for example, the [maketable()](../scorefile/maketable.html) scorefile
command. *index* designates which PField from the incoming p\[\]-array
(starting from 0, of course) holds the reference to the table, and
*\*tablelen* is an int variable passed into the method by reference; the
resulting value loaded into the *tablelen* variable will be the length
of the array.

  
  
<span id="getSkip"></span> **int Instrument::getSkip()**  
  

returns the value of the variable *\_skip*, which is set by the
[reset](../scorefile/reset.html) scorefile command. This is the number
of samples to count down for one 'control-rate' period. See the
[update](update.html) function for an example of how this might be used.
The default value is 1000.

  
  
<span id="increment"></span> **void Instrument::increment()**  
  

increments the value of *cursamp*, used to keep track of how many
samples (frames) have been executed. Equivalent to *++cursamp*.

  
  
**void Instrument::increment(***int amount***)**  
  

increments the value of *cursamp* by *amount*, used to keep track of how
many samples (frames) have been executed. Equivalent to *cursamp +=
amount*.

  
  
<span id="getstart"></span> **float Instrument::getstart()**  
  

returns the start time (in seconds) of the INSTRUMENT note. This is the
value of the variable *\_start*.

  
  
<span id="getdur"></span> **float Instrument::getdur()**  
  

returns the duration (in seconds) of the INSTRUMENT note. This is the
value of the variable *\_dur*.

  
  
<span id="getendsamp"></span> **int Instrument::getendsamp()**  
  

returns the ending sample number (frame) of the INSTRUMENT note. This is
the value of the variable *endsamp*.

  
  
<span id="setchunk"></span> **void Instrument::setchunk(***int
chunksize***)**  
  

sets the value of the *chunksamps* variable to *chinksize*. This is the
number of sample frames that will be processed during each invocation of
the **run** sample-computing method.

  
  
<span id="setendsamp"></span> **void Instrument::setendsamp(***int
sampno***)**  
  

sets the ending sample number (frame) of the INSTRUMENT note to
*sampno*. This sets the value of the variable *endsamp*.

  

-----

  
The following virtual methods can be overridden by the INSTRUMENT
designer in building an RTcmix instrument:  
  
<span id="init"></span> **int Instrument::init(***double p\[\], int
n\_args***)**  
  

is called by the parser to set up and schedule an INSTRUMENT for
execution. The *p\[\]* array contains the parameters for the INSTRUMENT
note passed in from the scorefile, and *n\_args* is the total number of
parameters parsed for that INSTRUMENT note. The maximum size of this
*p\[\]* array is set by

in the "RTcmix/src/include/maxdispargs.h" file. The number of samples to
be computed for the note (see the **[nSamps](#nSamps)** method) is
returned, or the token *DONT\_SCHEDULE* if there was an error in the
initialization of the INSTRUMENT note. See the RTcmix instrument design
[tutorial](../../tutorials/instrumentdesign.html) for more information
on this INSTRUMENT class method.

  
  
<span id="run"></span> **int Instrument::run()**  
  

is called during RTcmix execution to compute samples for each INSTRUMENT
note that is scheduled. It is expected that this method will produce
*chunksamps* samples (see the **[framesToRun](#framesToRun)** method)
each time it is called. Generally the number of samples computed is
returned. See the RTcmix instrument design
[tutorial](../../tutorials/instrumentdesign.html) for more information
on this INSTRUMENT class method.

  
  
<span id="configure"></span> **int Instrument::configure()**  
  

is sometimes called to set up sample buffers or initialize run-time
variables for specific INSTRUMENTs. **configure** is also called once at
the beginning of a scheduled note (except in the older 'rtinteractive
mode' using a TCP/IP socket interface), so it can be used to perform
one-time tasks that need to happen just as an INSTRUMENT begins a
note.  
  
0 is returned for a successful completion, -1 otherwise.

  

-----

  
Other INSTRUMENT class methods that are of general utility have their
own documentation entrries. These include:  
  

  - **[rtsetoutput](rtsetoutput.html)**  
  - **[rtsetinput](rtsetinput.html)**  
  - **[rtinrepos](rtinrepos.html)**  
  - **[rtgetin](rtgetin.html)**  
  - **[rtaddout](rtaddout.html)**  
  - **[rtbaddout](rtbaddout.html)**  
  - **[update](update.html)**

  

> *\[NOTE: There are a number of other methods associated with the
> INSTRUMENT class, but these work upon scheduling aspects of RTcmix
> execution that probably shouldn't be addressed within an instrument.
> However, if you need to do this kind of modification, please see the
> source files "RTcmix/src/rtcmix/Instrument.cpp" and
> "RTcmix/src/rtcmix/Instrument.h"\]*

-----

### Examples

oh there are many of them...
