---
title: CHAIN()
layout: ref
---

## CHAIN

Group instruments.

  

-----

##### quick syntax:

**CHAIN**(outsk, dur, number, handle1...n)

-----

  

```cpp
   p0 = output start time (seconds)
   p1 = duration (-endtime)
   p2 = number of instruments to follow
   p3-n = handles for instruments to be chained
```

-----

  
**CHAIN** runs a series of instruments as a group, chaining the output
of each to the input of the next. All the instruments are scheduled
together.

### Usage Notes

To add an instruments to **CHAIN**, you have to create the instruments
using the [makeinstrument()](../scorefile/makeinstrument.html) utility.
Use the handle that is returned as the argument for that instrument.
**CHAIN**'d instruments do not use mix buses between them, but you still
need to use bus\_config() to configure each instrument's input and
output channel counts. To do this, there is a special bus type called
"chain", which configures the instrument without invoking any of the in,
out, or aux bus logic. So, if you wished to place WAVETABLE and DELAY in
a **CHAIN**, you could configure each instrument like so:

```cpp
    // run WAVETABLE in monaural mode
    bus_config("WAVETABLE", "chain 0 out");
    // run DELAY 1-channel in, 2-channel out            
    bus_config("DELAY", "chain 0 in", "chain 1-2 out");
    // CHAIN's output MUST match output of last inst in chain (2-chan)
    bus_config("CHAIN", "out 0-1");
```

If the first instrument in the chain reads from disk, its input bus is
configured just the way it would be in an unchained system:

```cpp
    // read from file input, 2-channel out
    bus_config("TRANS", "in 0", "chain 0-1 out");
    // run DELAY 2-channel in, 2-channel out
    bus_config("DELAY", "chain 0-1 in", "chain 2-3 out");
    bus_config("CHAIN", "out 0-1");
```

In the same fashion as with aux busses, the outskip values for the
**CHAIN**'d instruments should all be 0. The **CHAIN** instrument alone
determines the output skip. The duration of the instruments can be
arbitrary, but will be truncated to the duration set in **CHAIN**. If
**CHAIN**'s duration is longer than its instruments, the extra time will
be filled with zeros.

### Sample Scores

very basic:

```cpp
    rtsetparams(44100, 2)
    load("ELL")
    load("TRANS");
    load("CHAIN");
    load("STEREO");

    rtinput("tube.wav")
    rtoutput("ell01.wav");
    inchan = 0
    inskip = 0
    dur = DUR() + 2

    ripple = 40
    atten = 90
    ringdur = 1.5;

    bus_config("TRANS3", "in 0", "chain 0 out");  // 1 in, 1 out
    bus_config("ELL", "chain 0 in", "chain 1 out");  // 1 in, 1 out
    bus_config("STEREO", "chain 1 in", "chain 2-3 out");  // 1 in, 2 out
    bus_config("CHAIN", "out 0-1");  // 2 out (must match last instrument in the chain)

    env = maketable("line", 1000, 0,0, .004,1, dur/2,1, dur,0)

    srand(9)

    start = 0;
    cfbase = 2000;
    cfswing = cfbase * .9;

    while (start < 5) {
        // set up elliptical filter
        pbcut = cfbase + irand(-cfswing, cfswing);
        sbcut = pbcut + irand(100, 300);
        ellset(pbcut, sbcut, 0, ripple, atten)

        amp = irand(0.5, 0.9);
        pan = random()

        transp = 1.1 + irand(-.09, .09);
        tdur = translen(dur, transp);

        trans = makeinstrument("TRANS3", 0, inskip, tdur, 1, transp);
        dur2 = tdur * 2;  // to allow filter to ring
        ell = makeinstrument("ELL", 0, 0, dur2, amp * env, ringdur);
        stereo = makeinstrument("STEREO", 0, 0, dur2, 1, pan);

        CHAIN(start, dur2, 3, trans, ell, stereo);
        start = start + (irand(0.03, 0.1));
        cfswing = cfswing * 0.99;
    }
```

  

-----

### See Also

[makeinstrument](../scorefile/makeinstrument.html)
