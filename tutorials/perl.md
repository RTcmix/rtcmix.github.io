---
title: Using Perl as the RTcmix Command-Language Interface
layout: ref
---

# Using Perl as the RTcmix Command-Language Interface

The default RTcmix application (invoked by the
[CMIX](../reference/interface/CMIX.html) command) uses the included
scorefile language [Minc](../reference/scorefile/Minc.html). However, if
you configure the build of RTcmix for Perl support using the
*--with-perl* flag (see the [installation
guide](../standalone/index.html) for information about this), then
compiling RTcmix will make a Perl-enabled command
([PCMIX](../reference/interface/PCMIX.html)) that has a Perl parser as
the 'front-end' instead of Minc.

## Using Perl/RTcmix

To use Perl with RTcmix, write a Perl script with the following
statement at the top:

    use RT;

Then use any Perl statements and any of the function calls you would use
in an RTcmix Minc script. Run the script with the command *PCMIX*
(instead of *CMIX)*.

Now, whenever a PCMIX script uses a function name unknown to Perl, Perl
will pass the function and its arguments to RTcmix. If it's unknown to
RTcmix, then RTcmix will give its usual error message about it and
continue processing the script.

This means that any RTcmix functions loaded into a script (with the load
command) will work, even if these belong to an instrument not part of
the distribution source tree. Some Minc functions have the same names as
internal Perl functions. In general, Perl versions take precedence over
RTcmix versions, with the following exceptions:

    srand()
    rand()
    reset()

If your script uses these names, you'll get the RTcmix versions. If you
want to use the Perl versions, you must use the *CORE::* prefix, like
this:

``` 
   CORE::srand(4);
   $foo = CORE::rand(99);
```

If you want to use the other RTcmix functions whose names collide with
Perl ones, then you must use the *RT::* prefix., like this:

``` 
   RT::print($foo);
```

The only such functions that you could possibly care about are print,
splice and open. But who wouldn't want to use the Perl print instead of
the RTcmix one?

## Sample Scorefiles

A simple [WAVETABLE](../reference/instruments/WAVETABLE.html) Perl
script:

    use RT;
    
    print "\nWelcome to the Perl front end to RTcmix!\n\n";
    
    rtsetparams(44100, 1);
    load("WAVETABLE");
    
    setline(0,0, 1,1, 4,1, 5,0);
    makegen(2, 10, 1000, 1, 0.3, 0.2);
    
    $start = 0;
    $dur = 0.7;
    $amp = 15000;
    $freq = 7.00;
    
    for ($i = 0; $i < 5; $i++) {
       WAVETABLE($start, $dur, $amp, $freq);
       $start += 0.5;
       $amp -= 2000;
       $freq += 0.07;
    }

Another [WAVETABLE](../reference/instruments/WAVETABLE.html) Perl
script:

    # This is a translation of sample_scos_1.0/WAVETABLE3.sco into Perl.   -JGG
    
    use RT;
    
    rtsetparams(44100, 2);
    load("WAVETABLE");
    print_off();
    makegen(1, 24, 1000, 0, 1,  950, 0);
    makegen(2, 10, 1000, 1, 0.3, 0.2);
    
    srand(0.35);
    
    for ($start = 0; $start < 7; $start += 0.14) {
       $freq = (random() * 200) + 35;
       for ($i = 0; $i < 3; $i += 1) {
          WAVETABLE($start, 0.4, 1500, $freq, 0);
          WAVETABLE($start + random()*0.1, 0.4, 1500, $freq + (random()*7), 1);
          if ($start > 3.5) {
             makegen(2, 10, 1000, 1, random(), random(), random(), random(),
                      random(), random(), random(), random(), random(), random(),
                      random());
          }
          $freq += 125;
       }
    }

Perl script showing the RTcmix ability to chain several instruments
together:

    # Quick translation of sample_scos_3.0/LONGCHAIN_1.sco to Perl, for testing.
    
    # This score makes a wavetable synth riff and feeds it through 3 effects
    # in series: flange -> delay -> reverb
    
    use RT;
    
    print_off();
    rtsetparams(44100, 2);
    load("WAVETABLE");
    load("FLANGE");
    load("JDELAY");
    load("REVERBIT");
    
    bus_config("WAVETABLE", "aux 0-1 out");
    bus_config("FLANGE", "aux 0-1 in", "aux 10-11 out");
    bus_config("JDELAY", "aux 10-11 in", "aux 4-5 out");
    bus_config("REVERBIT", "aux 4-5 in", "out 0-1");
    
    $totdur = 30;
    $masteramp = 1.0;
    $atk = 2; $dcy = 4;
    
    $numnotes = makegen(3, 2, 99,
                       5.00, 5.001, 5.02, 5.03, 5.05, 5.07, 5.069, 5.10, 6.00);
    $transposition = 2.00;   # try 7.00 also, for some cool aliasing...
    srand(2);
    
    
    # ---------------------------------------------------------------- synth ---
    $notedur = .10;
    $incr = $notedur + .015;
    
    $maxampdb = 92;
    $minampdb = 75;
    $ampdiff = $maxampdb - $minampdb;
    
    control_rate(20000);        # need high control rate for short synth notes
    setline(0,0, 1,1, 20,0);
    makegen(2, 10, 10000, 1, .9, .7, .5, .3, .2, .1, .05, .02);
    
    for ($st = 0; $st < $totdur; $st += $incr) {
       $slot = random() * $numnotes;
       $pitch = pchoct(octpch(sampfunc(3, $slot)) + octpch($transposition));
       $amp = ampdb($minampdb + ($ampdiff * random()));
       WAVETABLE($st, $notedur, $amp, $pitch, $pctleft=random());
    }
    
    
    # for the rest
    control_rate(500);
    $amp = $masteramp;
    
    # --------------------------------------------------------------- flange ---
    $resonance = 0.3;
    $lowpitch = 5.00;
    $moddepth = 90;
    $modspeed = 0.08;
    $wetdrymix = 0.5;
    $flangetype = 0;
    
    $gensize = 100000;
    makegen(2,10,$gensize, 1);
    
    $maxdelay = 1.0 / cpspch($lowpitch);
    
    setline(0,1,1,1);
    
    FLANGE($st=0, $insk=0, $totdur, $amp, $resonance, $maxdelay, $moddepth,
             $modspeed, $wetdrymix, $flangetype, $inchan=0, $pctleft=1);
    
    $lowpitch += .07;
    $maxdelay = 1.0 / cpspch($lowpitch);
    
    makegen(2,9,$gensize, 1,1,-180);
    
    FLANGE($st=0, $insk=0, $totdur, $amp, $resonance, $maxdelay, $moddepth,
             $modspeed, $wetdrymix, $flangetype, $inchan=1, $pctleft=0);
    
    
    # ---------------------------------------------------------------- delay ---
    $deltime = $notedur * 2.2;
    $regen = 0.70;
    $wetdry = 0.12;
    $cutoff = 0;
    $ringdur = 2.0;
    
    setline(0,0, $atk,1, $totdur-$dcy,1, $totdur,0);
    
    JDELAY($st=0, $insk=0, $totdur, $amp, $deltime, $regen, $ringdur, $cutoff,
             $wetdry, $inchan=0, $pctleft=1);
    JDELAY($st=0.02, $insk=0, $totdur, $amp, $deltime, $regen, $ringdur, $cutoff,
             $wetdry, $inchan=1, $pctleft=0);
    
    
    # --------------------------------------------------------------- reverb ---
    $revtime = 1.0;
    $revpct = .3;
    $rtchandel = .05;
    $cf = 0;
    
    setline(0,1, 1,1);
    
    REVERBIT($st=0, $insk=0, $totdur+$ringdur, $amp, $revtime, $revpct,
                $rtchandel, $cf);
    
    
    
    # john gibson, 17-june-00

FInally, a Perl script invoking a number of simultaneous notes:

    # Illustrates using a function to play one note consisting of 
    # multiple RTcmix notes.
    
    use RT;
    
    rtsetparams(44100, 2);
    load("WAVETABLE");
    
    # Set to 1 to write a sound file (if processor too slow for rt playback).
    $writeit = 0;
    
    if ($writeit) {
       set_option("audio_off", "clobber_on");
       rtoutput("/tmp/bell.aiff");
    }
    
    # just a sine wave (try extra harmonics for more complex bell sound)
    makegen(2, 10, 5000,  1);
    
    # exponential amplitude envelope
    makegen(1, 5, 1000,  1, 1000, .0005);
    
    # individual bell notes
    #    start dur    amp   freq
    bell(0,     5.0,  4000, 1000);
    bell(3,     8.0,  5000, 1200);
    bell(6,    12.0,  3000,  800);
    
    # bell notes in a loop (needs fast processor for real time playback)
    $incr = 0.1;
    for ($st = 10; $st < 20; $st += $incr) {
       $freq = (random() * 1000) + 400;
       bell($st, 8, 2000, $freq);
       $incr += 0.2;
    }
    
    
    # Play one bell note.  Parameters are start time, duration, amplitude,
    # and fundamental frequency.  (Based on description of Risset's bell
    # instrument in the Dodge book.)
    
    sub bell () {
       my($start, $dur, $amp, $freq) = @_;
       my $pan;
       print "fundamental frequency: $freq\n";
       WAVETABLE($start, $dur,         $amp,         $freq * .56,        $pan=1);
       WAVETABLE($start, $dur * .9,    $amp * .67,   $freq * .56 + 1,    $pan=0);
       WAVETABLE($start, $dur * .65,   $amp,         $freq * .92,        $pan=.9);
       WAVETABLE($start, $dur * .55,   $amp * 1.8,   $freq * .92 + 1.7,  $pan=.1);
       WAVETABLE($start, $dur * .325,  $amp * 2.67,  $freq * 1.19,       $pan=.8);
       WAVETABLE($start, $dur * .35,   $amp * 1.67,  $freq * 1.7,        $pan=.2);
       WAVETABLE($start, $dur * .25,   $amp * 1.46,  $freq * 2,          $pan=.7);
       WAVETABLE($start, $dur * .2,    $amp * 1.33,  $freq * 2.74,       $pan=.3);
       WAVETABLE($start, $dur * .15,   $amp * 1.33,  $freq * 3,          $pan=.6);
       WAVETABLE($start, $dur * .1,    $amp,         $freq * 3.76,       $pan=.4);
       WAVETABLE($start, $dur * .075,  $amp * 1.33,  $freq * 4.07,       $pan=.5);
    }
    
    
    # [scorefile by John Gibson]
