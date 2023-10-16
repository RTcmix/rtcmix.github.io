---
title: PField Control
layout: ref
---

### PField Control

Beginning in RTcmix 4.0, a new system for controlling Instrument
parameters was installed. This replaces the original
[makegen](/reference/scorefile/makegen.php) system for various
Instrument envelopes, although it is still supported for nearly all of
the older instruments. This new system also allows the user to control
Instrument parameters in real-time as a note is being executed. For
example, the pitch parameter of an Instrument might be "pfield-enabled".
This means that the pitch of an executing note could be controlled by an
RTcmix 'table' or it could be controlled by an external connection, so
that the pitch can be altered dynamically (perhaps in response to mouse
movement or an external controller) as the note is sounding.

PField control is accomplished using special RTcmix variables called
*pfield-handles* or *table-handles*. The parsers for RTcmix are
configured to allow a number of basic operations (arithmetic, etc.) on
these variables. There are also a number of specialized filters and
operator commands for these *handle* variables. PField-enabled
parameters can also be single values or 'regular' RTcmix variables, of
course.

The [Basic RTcmix Tutorial](/tutorials/standalone.php) has examples of
how this control system works, and the [RTcmix Instrument Design
Tutorial](/tutorials/instrument_design.php) discusses how to incorporate
PFIeld control into user-written Instruments.

See the [Short Tour of PField Capabilities](/tutorials/PFields.php)
tutorial for more information about various PField scorefile commands.  
  

------------------------------------------------------------------------

### SEE ALSO

[maketable](/reference/scorefile/maketable.php),
[makeconnection](/reference/scorefile/makeconnection.php),
[makemonitor](/reference/scorefile/makemonitor.php),
[makeLFO](/reference/scorefile/makeLFO.php),
[makefilter](/reference/scorefile/makefilter.php),
[makerandom](/reference/scorefile/makerandom.php)
