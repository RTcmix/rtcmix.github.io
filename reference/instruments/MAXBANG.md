---
title: MAXBANG()
layout: ref
---

## MAXBANG

Utility instrument used to generate a Max "bang" message.

*in rtcmix\~/iRTcmix: RTcmix/insts/maxmsp*  
  

-----

##### quick syntax:

**MAXBANG**(time)


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | time to generate the bang | seconds | no | no | 

   Author Brad Garton, 1/2004

  

-----

  
**MAXBANG** sends a 'bang' out the right outlet of the
[rtcmix\~](../../rtcmix_/index.html) object. It can also be used in
[iRTcmix](../../irtcmix/index.html) for iOS.

### Usage Notes

**MAXBANG** works by setting a flag when the **MAXBANG** note is
scheduled. An internal function, check\_bang(), determines if the flag
has been set. If it has, it produces a 'bang' at the right outlet of the
Max/MSP [rtcmix\~](../../rtcmix_/index.html) object.

**MAXBANG** can also be used in [iRTcmix](../../irtcmix/index.html)
for timing and scheduling purposes (the check\_bang() function is called
directly in the application developer code).

**MAXBANG** is limited by the vector or buffer size, so it is not
sample-accurate. Scheduling more then one 'bang' within a vector or
buffer will only result in one 'bang' at the vector/buffer boundary.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("MAXBANG")      // note:  the "load" is not necessary in rtcmix~/iRTcmix

   // send a bang 1.5 seconds after receiving this score
   MAXBANG(1.5)
```

  

-----

### See Also

[MAXMESSAGE](MAXMESSAGE.html), [rtcmix\~](../../rtcmix_/index.html),
[iRTcmix](../../irtcmix/index.html)
