---
title: Creating an RTcmix Instrument
layout: ref
---

# Creating an RTcmix Instrument

*\[NOTE: This tutorial covers writing version 3.x RTcmix instruments. A
number of changes have been made to instruments in v. 4.0 -- this
tutorial will be updated soon\! In the meantime, the basic principles
are still appropriate, but you should consult the file*
[README.inst\_porting\_v4](README.inst_porting_v4.html) *as well as the
"sample\_code" examples in the "RTcmix/docs" directory of the
distribution.\]*

Most RTcmix instruments are designed to inherit from the *Instrument*
class -- see the [Instrument class](../reference/design/Instrument.html)
documentation for a more-or-less complete listing of variables and
associated functions for this class. Don't be fooled by the relative
complexity of the *Instrument* class, however. As an instrument
designer, you are only required to implement two member functions for
your instrument. You will need to set up your instrument by filling out
the *init()* function, and you will need to write your sample-computing
code in the *run()* member function. That's all\!

When RTcmix runs, it first calls a particular instrument's *init()*
function whenever it detects that the instrument command has been
parsed. The *init()* function then initializes various parameters,
including setting the start time and end time for the note of that
instrument on the RTcmix scheduled execution queue (called "the heap").
When RTcmix starts running "the heap" and a note start is encountered,
then the *run()* function for that instrument is called repeatedly to
generate samples for output. Although RTcmix calls the instrument/note's
*run()* function in chunks corresponding to the *bufsamps* size that can
be set via [rtsetparams](../reference/scorefile/rtsetparams.html)
scorefile command, when you design an instrument you can safely assume
that the processing of samples in the *run()* function will be
continuous.

Sounds tricky? Actually, it's not too difficult at all. To show how all
this works, we'll design a simple oscillator instrument and then
progressively add features to build a more complex signal-processing
amplitude modulation (AM) instrument.

## A Simple Oscillator Instrument

What we're planning to do is to create a limited version of the
[WAVETABLE](../reference/instruments/WAVETABLE.html) instrument.
WAVETABLE works by reading a waveform created by the
[makegen](../reference/scorefile/makegen.html) scorefile command. For
reasons that will become apparent as we elaborate the instrument, we'll
assume that the function-table slot used for the *makegen* waveform will
be slot \#2. *\[note: In addition to the* makegen *command
documentation, click [here](standalone.html#makegen) for a short
discussion of how* makegen *can be used in a scorefile to create a
waveform.\]*

WAVETABLE, and hence our simple oscillator instrument, works by copying
values from the *makegen*-created function-table slot to the output in
such a manner that a repeating (oscillating\!) waveform with a specific
frequency, amplitude and duration is heard when the samples are
converted to sound.

This means that we have to make some decisions about how to control the
parameters of our simple oscillator. Already we have decided that the
waveform will be stored in function-table slot \#2. We also need to
define *p-fields* (parameters) for the start time, the duration, the
amplitude and the pitch. We'll simply do them in that order (remember
that in C++ numbering starts from 0):

We're also going to stipulate that the instrument will write nothing but
2-channel (stereo) output sound, with the amplitude being equal in both
channels. *\[note: By convention, p0 is generally the start time for an
RTcmix instrument. p1 is usually the duration, unless the instrument is
a signal-processing instrument. Then p1 represents the amount to skip on
an input soundfile for processing, and p2 then becomes the duration
parameter. The rest of the p-fields are deteremined by the instrument
designer.\]*

FInally, we need to decide what to call our instrument. SIMPLEOSC seems
like an appropriate, although somewhat uninspired name.
<span id="template"></span>

## Setting up the Template

If we had to write <u>everything</u> in an RTcmix instrument from
scratch, it would be tedious and difficult indeed. Fortunately there is
a Better Way. In the "RTcmix/docs/sample\_code" directory (in the RTcmix
distribution) as a directory titled "TEMPLATE" (click
[here](TEMPLATE.tar.html) to download a "TEMPLATE.tar.gz" file that will
unpack to this directory). Copy the entire "TEMPLATE" directory over to
the location where you plan to work on building SIMPLEOSC. Rename the
"TEMPLATE" directory "SIMPLEOSC". Inside the "SIMPLEOSC" directory you
should still see files like "TEMPLATE.cpp" and "TEMPLATE.h". Follow
these easy-as-pie directions, and you will be ready to go to work on the
SIMPLEOSC instrument:

1\. Rename the files "TEMPLATE.cpp" and "TEMPLATE.h" to "SIMPLEOSC.cpp"
and "SIMPLEOSC.h"  
2\. Edit the file "SIMPLEOSC.cpp" and change <u>every</u> occurence of
the word *TEMPLATE* to *SIMPLEOSC*.  
3\. Edit the file "SIMPLEOSC.h" and change <u>every</u> occurence of the
word *TEMPLATE* to *SIMPLEOSC*.  
4\. Edit the file "Makefile" and change the line:

```cpp
NAME = TEMPLATE
```

to

```cpp
NAME = SIMPLEOSC
```

You are now set to design and build SIMPLEOSC.

## The SIMPLEOSC::init() Member Function

Edit the file "SIMPLEOSC.cpp" again. Locate the definition of the
*SIMPLEOSC::init()* function (it should be about line 15 in the file).
Note that *init()* is passed two variables -- the first is an array of
floating-point numbers (*p\[\]*) and the second is an integer variable
(*n\_args*). The values in these variables is determined by the
scorefile. If RTcmix parses the following line in a scorefile:

```cpp
SIMPLEOSC(0, 3.5, 20000, 478.0)
```

then the *SIMPLEOSC::init()* function will be called with *n\_args* set
to 4 and the *p\[\]* array set to these values:

```cpp
p[0] = 0.0;
p[1] = 3.5;
p[2] = 20000.0;
p[3] = 478.0;
```

This is how data passes into instruments for particular notes from an
RTcmix scorefile. Note that all of the p-field parameters are converted
to floating-point, even if they were entered as integers in the
scorefile. That's just the way it is.

We won't be using *n\_args* in SIMPLEOSC, but it is useful for checking
how many p-fields are actually present for error-checking, setting up
optional p-fields, etc. The default maximum p-fields in RTcmix is set at
1024. This can be changed by finding the definition for *MAXDISPARGS* in
the file "RTcmix/H/mixdispargs.h", changing it to the desired value and
recompiling RTcmix.

The first thing we need to do is to set the start time and duration for
the instrument/note. This is done using the
[rtsetoutput](../reference/design/rtsetoutput.html) function.
*rtsetoutput()* takes 3 parameters, a starting time, a duration (both in
seconds), and a pointer to the instrument being called (represented in
C++ by the variable *this*. *rtsetoutput()* also returns how many sample
<u>frames</u> we will compute. *\[note: a sample <u>frame</u>
corresponds to one 'sample' of time, irregardless of how many channels
are in the output. For a 1-channel output, this is just the total number
of samples to be computed. For a stereo output, this is 1/2 the total
number of samples to be computed, because we need to compute 2 samples
for each 'sample' of time. This is not something you will necessarily
have to worry about.\]* We will store the returned number of frames in
the *Instrument* class variable *nsamps*. *p\[0\]* is our start time and
*p\[1\]* is our duration, so the first addition we will make to the
*SIMPLEOSC::init()* function will be:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
    // p0 = start, p1 = duration

    nsamps = rtsetoutput(p[0], p[1], this);

    ...
```

This is such a common operation in RTcmix instrument design that we have
already set it up in our TEMPLATE instrument directory.

Next we need to store the amplitude into a variable that we can access
during the *SIMPLEOSC::run()* function, a variable that we will use to
multiply the sample values we generate to reach a specified amplitude.
We will call the variable *amp* and the addition to the
*SIMPLEOSC::init()* function to transfer the value from *p\[2\]* to the
variable is trivial:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
      // p0 = start, p1 = duration, p2 = amplitude

      nsamps = rtsetoutput(p[0], p[1], this);

      amp = p[2];

...
```

*amp*, however, is not an *Instrument* class variable, so we need to
declare it. Since *amp* will be used in both the *SIMPLEOSC::init()* and
*SIMPLEOSC::run()* functions, we need to declare it so that both of
these SIMPLEOSC functions have access to it. This is done by putting the
declaration for *amp* in the "SIMPLEOSC.h" header file:

```cpp
class SIMPLEOSC : public Instrument {
      float amp;

public:
      SIMPLEOSC();

...
```

Notice that *amp* is declared as type "float". Sample values in RTcmix
are indeed floating-point ("float") numbers; since *amp* will be used to
multiply samples, then it makes sense to declare it as the same type.

All that is left for us to do in initializing our instrument is to set
up a wavetable oscillator to work with the proper frequency *p\[3\]*and
the proper waveform (function-table slot \#2). In the Bad Old Days, this
used to be rather annoying, involving calculations of a weird thing
called a sampling increment and strange setups of variables for the
phase of the oscillator, etc. (you will still run into this kind of code
in many of the RTcmix instruments included in the distribution). RTcmix
now provides a handy object, [Ooscili](../reference/design/Ooscil.html)
that makes our job much easier. All we have to do is instantiate the
object with the desired frequency and function-slot table \#:

```cpp
       int SIMPLEOSC::init(float p[], int n_args)
       {
              // p0 = start, p1 = duration, p2 = amplitude, p3 = frequency

              nsamps = rtsetoutput(p[0], p[1], this);

              amp = p[2];

              theOscil = new Ooscili(p[3], 2);

       ...
```

*Ooscili* takes a floating-point value in Hz for frequency, and an
integer for the function-table slot of the waveform to use. Of course,
we have to declare *theOscil*, this is also done in "SIMPLEOSC.h"
because it will be used in both *SIMPLEOSC::init()* and
*SIMPLEOSC::run()*:

```cpp
class SIMPLEOSC : public Instrument {
      float amp;
      Ooscili *theOscil;

public:
      SIMPLEOSC();

...
```

All we have to do now is to return how many sample frames we need to
compute for the note, and our definition of the *SIMPLEOSC::init()* is
complete:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
      // p0 = start, p1 = duration, p2 = amplitude, p3 = frequency

      nsamps = rtsetoutput(p[0], p[1], this);

      amp = p[2];

      theOscil = new Ooscili(p[3], 2);

      return(nsamps);
}
```

## The SIMPLEOSC::run() Member Function

Now that we've set the initialization of our SIMPLEOSC instrument, we
need to create the sample-computing part in the *SIMPLEOSC::run()*
function. There are several items that we will need to establish in this
function that have already been set up in the TEMPLATE directory. The
first is that we will need to create a loop that will generate one
sample value every time it gets iterated. This appears as the skeleton
for a *for* loop already in the *SIMPLEOSC::run()* function:

```cpp
      for (i = 0; i < framesToRun(); i++) {
      ...
      }
```

The variable *i* has already been declared (locally, it is only used in
the *SIMPLEOSC::run()* function) and the *frameToRun()* function will
return how many samples we need to compute for our note.

The second item is that we need to let RTcmix keep track of how many
samples we have computed for our instrument/note. This is done with the
*increment()* function inside the sample-computing *for* loop. We also
need to tell RTcmix to "do stuff" necessary for sample output. Prior to
the *for* loop is the statment *Instrument::run()*. This will cause
RTcmix to "do" that "stuff". Our *SIMPLEOSC::run()* function now looks
like this:

```cpp
int SIMPLEOSC::run()
{
      int i;

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {

      ...
             increment();
      }

...
```

All we need to do now is to get our *Ooscili* object (*theOscil* in our
SIMPLEOSC instrument) to generate samples within the *for*
sample-computing loop. So easy\! We need to declare a variable that will
temporarily store the sample values prior to sending them out to the
Real World. For this task we will use an array with 2 elements:
*out\[2\]*. It will be a floating-point array, of course, since we will
be using it for samples, and *out\[0\]* will hold the value for channel
0 ("left") and and *out\[1\]* will hold the value for channel 1
("right).

The way to get *theOscil* to produce samples is to use the *next()*
member function of the *Ooscili* object. *Ooscili* puts out sample
values between -1.0 and 1.0, so we want to scale them (multiply them) by
*amp* in order to get our desired amplitude. Putting this all together,
we have the following:

```cpp
int SIMPLEOSC::run()
{
      int i;
      float out[2];

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {
             out[0] = theOscil->next() * amp;
             out[1] = out[0];

             increment();
      }

...
```

Why did we say "out\[1\] = out\[0\]"? Why didn't we do:

```cpp
     out[0] = theOscil->next() * amp;
     out[1] = theOscil->next() * amp;
```

The problem with the above is that <u>every</u> time the *next()*
function is called, it generates the next sample in the evolving
waveform. By assigning it once to *out\[0\]* and then moving to the next
sample and assigning it to *out\[1\]*, we will have effectively split
the waveform between the two channels, generating samples twice as fast
as we actually wanted. The sonic result is a sound twice the frequency
we desired, with a bit of grunginess potentially thrown in to boot.

Why even bother with assiging sample values to the *out* array anyhow?
This is because we use *out* to shuttle samples into our output stream
that gets sent to the digital-to-analog convertors in the computer. We
do this by using the [rtaddout](../reference/design/rtaddout.html)
function:

```cpp
     rtaddout(out);
```

Guess what? We're done\! We've now designed a
fully-fledged-and-functional RTcmix instrument. Adding a return value to
the *SIMPLEOSC::run()* function, our whole listing of
*SIMPLEOSC::init()* and *SIMPLEOSC::run()* looks like this:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
      // p0 = start, p1 = duration, p2 = amplitude, p3 = frequency

      nsamps = rtsetoutput(p[0], p[1], this);

      amp = p[2];

      theOscil = new Ooscili(p[3], 2);

      return(nsamps);
}


int SIMPLEOSC::run()
{
      int i;
      float out[2];

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {
             out[0] = theOscil->next() * amp;
             out[1] = out[0];

             rtaddout(out);

             increment();
      }

      return i;
}
```

The corresponding declarations in the "SIMPLEOSC.h" file are:

```cpp
class SIMPLEOSC : public Instrument {
      float amp;
      Ooscili *theOscil;

public:
      SIMPLEOSC();

...
```

All of the other text in the "SIMPLEOSC.C" and "SIMPLEOSC.h" files
should be left intact -- all of the *\#include*'s, the function
declarations for *makeSIMPLEOSC()*, etc. These are needed by RTcmix to
properly build the instrument.

## Compiling and Running *SIMPLEOSC*

We've already modified the Makefile to compile *SIMPLEOSC*. All you
should have to do at this point is say:

```cpp
make
```

and the Makefile should build the dynamically-loaded instrument library
"libSIMPLEOSC.so". Be sure that the file "package.conf" is set to list
your installation of RTcmix. The default contents of this file:

```cpp
include /usr/local/src/RTcmix/makefile.conf
```

will work if in fact you have installed RTcmix in
"/usr/local/src/RTcmix".

Next we need to create a scorefile to run SIMPLEOSC. Given the small
number of p-fields and constraints we have placed on the parameters in
the design of our instrument, this scorefile is very simple, indeed.
We'll create a text file, "S1.sco", with the following contents:

```cpp
rtsetparams(44100, 2)
load("./libSIMPLEOSC.so")

makegen(2, 10, 1000, 1.0, 0.3, 0.1)
SIMPLEOSC(0, 3.5, 20000, 387.14)
```

Notice that the *load* scorefile command is instructed to load
*"./libSIMPLEOSC.so"* instead of *"SIMPLEOSC"*. This signals *load* to
search in the local directory ("./") instead of the default main RTcmix
shared library ("/usr/local/src/RTcmix/shlib").

Running the CMIX command with this score:

```cpp
CMIX < S1.sco
```

should yield a Glorious 387.14 Hz sound with an amplitude of 20000 and a
duration of 3.5 seconds. Yay\!

## Adding an Amplitude Envelope

As much fun as SIMPLEOSC is, the start and end of each note is a bit
stark. Our next step is to add an amplitude envelope, or a way of fading
up and down the sample amplitude as we generate each sample. Here is the
code in the *SIMPLEOSC::run()* member function that does in fact produce
the samples:

```cpp
	out[0] = theOscil->next() * amp;
```

What we need to do is obvious -- we have to find a way of dynamically
altering *amp* to accomplish the fade-up/fade-down.

One approach would be to install some simple math that would accompish
this, but there is another approach that will give us a lot more
flexibility in designing complicated amplitude envelopes. Recall that
*amp* is set to be the maximum sample amplitude that we want to generate
for each note (using the 0-32768 16-but integer scale). If we can
generate an additional multiplier that tracks a curve from 0 to 1, then
we can use that to modify the overall value of *amp*. Finding a way to
generate a 0-1 curve of some kind will give us a very powerful tool to
build an amplitude envelope.

[makegen](../reference/scorefile/makegen.html) to the rescue again\! A
number of *makegen* routines are in fact designed to do exactly this.
[gen24](../reference/scorefile/gen24.html),
[gen18](../reference/scorefile/gen18.html),
[gen4](../reference/scorefile/gen4.html),
[gen5](../reference/scorefile/gen5.html),
[gen6](../reference/scorefile/gen6.html) and
[gen7](../reference/scorefile/gen7.html) can all be used to make curves
between 0.0 and 1.0 of almost arbitrary complexity. All we need to do is
read the values from these *makegen*-created functions and multiply them
by the *amp* variable.

We already know how to do this -- use the *Ooscili* object. The trick is
to set it up so that the amplitude envelope "oscillator" will only
"oscillate" once during the note duration. This is easy, just set the
oscillator frequency to *1.0/duration* (wavelength \[duration\] and
frequency are reciprocals).

So in the "SIMPLEOSC.h" header file we declare another *Ooscili*
variable:

```cpp
class SIMPLEOSC : public Instrument {
      float amp;
      Ooscili *theOscil;
      Ooscili *theEnv;

public:
      SIMPLEOSC();

...
```

and we initialize it in the *SIMPLEOSC::init()* member function:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
      // p0 = start, p1 = duration, p2 = amplitude, p3 = frequency

      nsamps = rtsetoutput(p[0], p[1], this);

      amp = p[2];

      theOscil = new Ooscili(p[3], 2);
      theEnv = new Ooscili(1.0/p[1], 1);

      return(nsamps);
}
```

Notice that we are using function-table slot \#1 for our amplitude
envelope curve. We originally chose function-table slot \#2 as our
oscillator waveform to allow this. Amplitude envelopes in RTcmix are
typically stored in function-table slot \#1 (the
[setline](../reference/scorefile/setline.html) scorefile command expects
this).

Using *theEnv* to shape our amplitude evolution is trivial:

```cpp
	out[0] = theOscil->next() * amp * theEnv->next();
```

So our modified SIMPLEOSC now looks like this:

```cpp
int SIMPLEOSC::init(float p[], int n_args)
{
      // p0 = start, p1 = duration, p2 = amplitude, p3 = frequency

      nsamps = rtsetoutput(p[0], p[1], this);

      amp = p[2];

      theOscil = new Ooscili(p[3], 2);
      theEnv = new Ooscili(1.0/p[1], 1);

      return(nsamps);
}


int SIMPLEOSC::run()
{
      int i;
      float out[2];

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {
             out[0] = theOscil->next() * amp * theEnv->next();
             out[1] = out[0];

             rtaddout(out);

             increment();
      }

      return i;
}
```

The declarations in the "SIMPLEOSC.h" file are:

```cpp
class SIMPLEOSC : public Instrument {
      float amp;
      Ooscili *theOscil;
      Ooscili *theEnv;

public:
      SIMPLEOSC();

...
```

Compiling this code will give you a versatile and powerful SIMPLEOSC,
indeed. How versatile and powerful? Theoretically, armed with a sine
wave and the amplitude controls in the modified SIMPLEOSC, you should be
able to recreate [any sound
imaginable\!](http://www.sosmath.com/fourier/fourier1/fourier1.html). Of
course, theory and practice are two different things...

Seriously, you can create some wonderful sounds using SIMPLEOSC. Used
with the [RTcmix scorefile](../reference/scorefile/index.html)
capabilities, SIMPLEOSC can build interesting granular-synthesis
textures. Try the following scorefile using SIMPLEOSC just for fun:

```cpp
rtsetparams(44100, 2)
load("./libSIMPLEOSC.so")

makegen(1, 24, 1000, 0.0,0.0, 0.1,1.0, 0.2,0.0)
makegen(2, 10, 1000, 1.0, 0.3, 0.1)

start = 0.0
basefreq = 200.0
for (i = 0; i < 1500; i=i+1)
{
      freq = irand(basefreq, basefreq+300)
      SIMPLEOSC(start, 0.2, 4000, freq)

      start = start + 0.01
      basefreq = basefreq + 2.0
}
```

## Reading an Input Soundfile

Our SIMPLEOSC instrument can be changed into a signal-processing
instrument with a few minor changes. By signal-processing, we mean that
it will operate upon an input sound instead of generating a sound 'from
scratch'. We'll call this instrument SIMPLEMIX -- we can set it up the
same way we did for SIMPLEOSC, using the directions for [**Setting up
the Template**](#template) (using SIMPLEMIX in place of SIMPLEOSC or we
can just modify SIMPLEOSC so that it becomes SIMPLEMIX.

In any case, we want to take out the references to the sample-generating
*Ooscili* object *theOsc*, but keep the amplitude *Ooscili* (*theEnv*).
In place of *theOscil*, we will set up a soundfile for reading using
[rtsetinput](../reference/design/rtsetinput.html). *rtsetinput* works
like [rtsetoutput](../reference/design/rtsetoutput.html), but it takes
only a *start time* parameter (along with the *Instrument\** pointer)
instead of a *start time* and *duration* parameter. This is because the
duration is established by *rtsetoutput*, having *rtsetinput* also do it
would be redundant. Thus *rtsetinput* does not return the number of
sample frames to be computed like *rtsetoutput*. Instead, it returns "0"
if it successfully opens the input, or "-1" if it doesn't. This is
useful for checking if a soundfile was opened properly, or if there was
some error (perhaps the name of the soundfile was mis-typed). Generally
errors will be caught in opening the input soundfile, accomplished
through the most recent call to the
[rtinput](../reference/scorefile/rtinput.html) scorefile command (this
establishes what *rtsetinput* will read).

The *start time* parameter for *rtsetinput* refers to the time-point (in
seconds) to start reading the input soundfile. This allows us to skip
into the input soundfile by a specified amount to process a particular
patch of sound. This parameter is ignored if the controlling scorefile
for the instrument sets up real-time ("live") input, as it is difficult
to skip arbitrarily into the future. It would probably be fun, though.

We'll modify our p-fields to accommodate this new input-skipping
parameter. We also need to initialize an
[Ortgetin](../reference/design/Ortgetin.html) object to deliver samples
into our *SIMPLEMIX::run()* method. As with *theEnv* (and *theOscil* in
SIMPLEOSC), we will declare this in our "SIMPLEMIX.h" file.

With these changes, our finished *SIMPLEMIX::init()* member function
becomes:

```cpp
int SIMPLEMIX::init(float p[], int n_args)
{
      // p0 = start, p1 = input skip, p2 = duration, p3 = amplitude

      nsamps = rtsetoutput(p[0], p[2], this);
      rtsetinput(p[1], this); // we're being bad, not checking for errors
      theIn = new Ortgetin(this);

      amp = p[3];

      theEnv = new Ooscili(1.0/p[2], 1);

      return(nsamps);
}
```

and "SIMPLEMIX.h" is:

```cpp
class SIMPLEMIX : public Instrument {
      float amp;
      Ortgetin *theIn;
      Ooscili *theEnv;

public:
      SIMPLEMIX();

...
```

The *SIMPLEMIX::run()* is then easy to code. All we have to do is
declare an input array that we will use to hold our incoming samples for
each frame, reading new samples into it using the *Ortgetin::next()*
method:

```cpp
int SIMPLEMIX::run()
{
      int i;
      float out[2];
      float in[2];
      float aamp;

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {
             theIn->next(in);
             aamp = amp * theEnv->next();
             out[0] = in[0] * aamp;
             out[1] = in[1] * aamp;

             rtaddout(out);

             increment();
      }

      ...
```

Notice that we are using the variable *aamp* to store the
envelope-shaped amplitude. This is to avoid calling *theEnv-\>next()*
more than once for each sample frame, and it also slightly increases the
efficiency of the instrument. As with SIMPLEOSC, also be aware that we
have made some assumptions about the nature of our input and output
soundfiles or devices -- they both need to be at least two-channels.
This instrument will not work properly for mono soundfiles\! If your
intention is to write a more general-purpose instrument capable of
handling mono/stereo/whatever soundfiles, then you will need to add code
to check for and handle differing channel configurations. *Ortgetin*
does not do this for you.

Given these caveats, our finished SIMPLEMIX instrument is:

```cpp
int SIMPLEMIX::init(float p[], int n_args)
{
      // p0 = start, p1 = input skip, p2 = duration, p3 = amplitude

      nsamps = rtsetoutput(p[0], p[2], this);
      rtsetinput(p[1], this); // we're being bad, not checking for errors
      theIn = new Ortgetin(this);

      amp = p[3];

      theEnv = new Ooscili(1.0/p[2], 1);

      return(nsamps);
}

int SIMPLEMIX::run()
{
      int i;
      float out[2];
      float in[2];
      float aamp;

      Instrument::run();

      for (i = 0; i < framesToRun(); i++) {
             theIn->next(in);
             aamp = amp * theEnv->next();
             out[0] = in[0] * aamp;
             out[1] = in[1] * aamp;

             rtaddout(out);

             increment();
      }

      return i;
}
```

As with SIMPLEOSC, we can use SIMPLEMIX to create some nifty granular
effects. The following scorefile, for example:

```cpp
rtsetparams(44100, 2)
load("./libSIMPLEMIX.so")

rtinput("/snd/somesound.aiff")

makegen(1, 24, 1000, 0.0, 0.0, 0.1, 1.0, 0.2, 0.0)

totaldur = DUR()
start = 0.0

for (i = 0; i < 100; i = i+1)
{
      inskip = irand(0.0, (totaldur-0.2))
      SIMPLEMIX(start, inskip, 0.2, 1)
      start = start + 0.1
}
```

will randomly grab little chunks of sound throughout a soundfile, while
this scorefile:

```cpp
rtsetparams(44100, 2)
load("./libSIMPLEMIX.so")

rtinput("/snd/somesound.wav")

makegen(1, 25, 1000, 1)

start = 0.0
inskip = 0.0

for (i = 0; i < 500; i = i+1)
{
      SIMPLEMIX(start, inskip, 0.1, 1)
      start = start + 0.05
      inskip = inskip + 0.02
}
```

will do a rough granular time-stretching of the original sound.
