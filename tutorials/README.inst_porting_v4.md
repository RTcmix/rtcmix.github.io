---
title: README.inst_porting_v4
layout: ref
---

    If your instrument works with RTcmix 3.8, and you can't compile
    it with 4.0, here are some things you must fix.  Do these before
    trying to support the new PField capabilities, discussed below.
    
    If you're trying to write a new instrument, model it on the sample
    code provided in docs/sample_code.
    
    
    1. The init method signature is now:
    
          virtual int INST::init(double p[], int n_args);
    
       Note: double, not float.  If you use float, you will see this error
       message at run time:
    
          "You haven't defined an init member of your Instrument class!"
    
    2. The configure, init and run methods should be declared virtual in INST.h.
    
    3. Do not call the base class configure method from your configure method.
       Do not call the base class init method from your init method.
       Do not call the base class run method from your run method.
    
    4. floc() now returns a double (not float) array, and genlib functions
       like oscili and tablei take a double array.  Please don't write a
       new instrument using floc.  It will still work, but we're trying to
       move away from makegens.
    
    5. Many Instrument base class variables that were protected are now private,
       so that you can't access them.  Use the inline accessor functions instead.
    
       Read only...
    
          base class vars      accessor function
          ---------------------------------------
          inputchans           inputChannels()
          outputchans          outputChannels()
          cursamp              currentFrame()
          nsamps               nSamps()
          chunksamps           framesToRun()
    
       Write...
    
          base class vars      accessor function
          ---------------------------------------
          cursamp++            increment()      // at end of run method loop
          cursamp += amount    increment(amount)
    
       These are the ones typically used in old instruments.  There may
       be others that crop up from time to time.  See src/rtcmix/Instrument.h
       for the accessors to use when your code tries to use a private base
       class variable.
    
    6. rtsetinput and rtsetoutput now return status (0: okay, -1: error) rather
       than nsamps.  The way to handle this is to use the following idiom.
    
          if (rtsetoutput(outskip, dur, this) == -1)
             return DONT_SCHEDULE;
          if (rtsetinput(inskip, this) == -1)
             return DONT_SCHEDULE;
    
    7. Use a configure method (as in MIX.cpp) whenever your inst takes input.
       Not required, but recommended over the old way of handling memory
       allocation in the run method.  Whatever you do, don't allocate lots of
       memory in the init method, because if a score plays many notes, they
       *all* will allocate memory at the start of the run, possibly resulting
       in an insane memory footprint.
    
    9. More recommended changes...  When computing values at the control rate,
       many insts use this idiom in the run method:
    
             if (branch < 0) {
                // update parameters
                branch = skip;
             }
    
       Now the Instrument base class initializes and maintains the skip value,
       so you should write this instead...
    
             if (branch <= 0) {
                // update parameters
                branch = getSkip();
             }
    
       Notice the "<=" in place of the "<".  We discovered that the old way meant
       that reset(44100) in a script would not do what you expect; instead it
       effectively set the control rate to 22050.  The new test fixes this.
    
       Also, make branch a data member, not a variable local to the run method.
       In the latter case, the control rate is not decoupled completely from
       the rtsetparams buffer size.  (It should be.)
    
    
    -------------------------------------------------------------------------------
    To support the new dynamic PField capabilities, you need to do these things:
    
    1. Decide which pfields can be changed while the note plays.
    
    2. Create a INST::doupdate() method that will update these values.  The
       simplest kind should look like this.  Declare this as a private member
       function in your INST.h file.
    
       void INST::doupdate()
       {
          double p[5];      // assuming 5 is number of pfields passed to your inst
          update(p, 5);     // fills p[] with updated values.
    
          amp = p[3];       // if amp is 4th pfield, as with an inst taking input
       }
    
    3. Call doupdate from your run loop, in the control-rate block.
    
       for (int i = 0; i < framesToRun(); i += inputChannels()) {
          if (branch <= 0) {
             doupdate();
             branch = getSkip();
          }
          // use updated values to make a cool signal
          ...
       }
    
    
    Consult docs/sample_code for simple examples of the technique.  If you 
    need to use a table directly, such as a wavetable, look at WAVETABLE
    for an example of how to do this.
    
    
    
    JGG, 6/17/05
