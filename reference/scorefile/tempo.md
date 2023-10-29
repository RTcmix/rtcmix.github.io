---
title: tempo()
layout: ref
---

## tempo

Create a tempo map for a score.

-----

### Synopsis

**tempo**(*beat0*, *tempo0* [,*beatX*, *tempoX*, ...])

*real\_time* = **tb**(*current\_beat*)

*current\_beat* = **bt**(*actual\_time*)

-----

### Description

**tempo** creates a map which allows you to convert arbitrary beat positions in a score into tempo-scaled time values and vice versa.  This in combination with the two conversion utilities, **tb** and **bt**, allow all the "timed" commands (i.e. calls to instruments, etc.) to be scaled to match the requested tempo.  

-----

### Arguments

  - *beat0*, *tempo0* ,*beatX*, *tempoX*, ...
  
    Pairs of beat locations and tempos.  Only the first pair are required.  Beats are an arbitrary unit; you can use any integer value as your "beat".  Tempos are in beats per minute.  The tempo will ramp linearly between the values of each consecutive beat pair.  To produce an instantaneous change, specify the same beat value for two consecutive tempo values.

	Specifying the beat points out of ascending order will generate an error.

-----

### Examples

```cpp 
// Start at tempo 120, keep constant for 48 beats, then slow to 82 over 16 beats

tempo(0, 120, 48, 120, (48+16), 82)

for (beat = 0; beat < 64; beat += 2)  // play a note every other beat
{
	// outskip will be set to the scaled times for each beat
	SOMEINSTRUMENT(outskip=tb(beat), dur=1, ...)
}

printf("total duration in seconds: %f\n", tb(beat-2) + dur)
```
```cpp
// Start at tempo 72 for 32 beats, then jump instantly to 96

tempo(0, 72, 32, 72, 32, 96)
for (beat = 0; beat < 64; beat += 2)  // play a note every other beat
{
	// outskip will be set to the scaled times for each beat
	SOMEINSTRUMENT(outskip=tb(beat), dur=1, ...)
}
```

-----

### See Also
[Minc](Minc.html)