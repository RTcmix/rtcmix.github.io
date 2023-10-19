---
title: pchcps()
layout: ref
---

**pchcps** - convert frequency to octave.pitch-class

-----

### Synopsis

**pchcps** *frequency*

-----

### Description

Given a frequency in Hz (cycles per second), **pchcps** returns that
value expressed in \`\`octave.pitch-class'' (*oct.pc*) notation.
*oct.pc* is a way to use standard "western" keyboard notes without
having to look up the pitch-frequency conversion. It works by
arbitrarily assigning the octave of middle-C to 8.00. Any semitone above
middle-C is added as a "hundredth" to the left of the decimal point,
i.e. 8.01 is the C\# just above middle-C, 8.02 is the D, 8.03 is the D\#
(Eb), etc. up to 8.12, which is equivalent to 9.00. 9.01 is then the C\#
one octave and a semitone abouve midddle-C.

The fun thing about this notation is that you are not limited to
keyboard-notes. A pitch specification of 7.07542389 will select a
frequency that is somewhere about half-way between the G (7.07) and Ab
(7.08) just below middle-C. Different RTcmix instruments will require
the pitch or frequency to be specified in different ways, although the
scorefile commands [cpspch](../scorefile/cpspch.html),
[pchcps](../scorefile/pchcps.html) or other related commands can do most
necessary conversions.
