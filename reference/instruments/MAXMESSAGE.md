---
title: MAXMESSAGE()
layout: ref
---

## MAXMESSAGE

Utility instrument used to output values in Max. 

*in rtcmix\~/iRTcmix: RTcmix/insts/maxmsp*  
  

-----

##### quick syntax:

**MAXMESSAGE**(time, val1, val2, ... valn)


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | time to send out the values | (seconds) | no | no | 
p1-pn | the values to be sent |  -  | no | no | 

   Author Brad Garton, 1/2004

  

-----

  
**MAXMESSAGE** will send a list of values out the right outlet of the
[rtcmix\~](../../rtcmix_/index.html) object. It can also be used in
[iRTcmix](../../iRTcmix/index.html) for iOS.

### Usage Notes

**MAXMESSAGE** works by setting a flag when the **MAXMESSAGE** note is
scheduled. An internal function, check\_vals(), determines if the flag
has been set. If it has, it sends a list of the values set in the
**MAXMESSAGE** note pfields out the right outlet of the Max/MSP
[rtcmix\~](../../rtcmix_/index.html) object.

**MAXMESSAGE** can also be used in
[iRTcmix](../../iRTcmix/index.html). (the check\_vals() function is
called directly in the application developer code).

**MAXMESSAGE** is limited by the vector or buffer size, so it is not
sample-accurate. Scheduling more then one **MAXMESSAGE** within a vector
or buffer will only send out the values in the most-recently-scheduled
**MAXMESSAGE** note.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("MAXMESSAGE")      // note:  the "load" is not necessary in rtcmix~/iRTcmix

   // send a list of values immediately after receiving this score
   MAXMESSAGE(0, 1.414, 7.8, 97)
```

  

-----

### See Also

  
[MAXBANG](MAXBANG.html), [rtcmix\~](../../rtcmix_/index.html),
[iRTcmix](../../iRTcmix/index.html)
