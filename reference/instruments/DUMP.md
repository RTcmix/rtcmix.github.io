---
title: DUMP()
layout: ref
---

## DUMP

Print pfield variable data (utility instrument).

*in RTcmix/insts/std*  
  

-----

##### quick syntax:

**DUMP**(outsk, dur, AMP\[, TABLE\])

CAPITALIZED parameters are [pfield-enabled](pfield-enabled.html) for
table or dynamic control (see the
[maketable](../scorefile/maketable.html) or
[makeconnection](../scorefile/makeconnection.html) scorefile
commands). Parameters after the \[bracket\] are optional and default to
0 unless otherwise noted.


Param Field	| Parameter | Units | Dynamic | Optional | Notes
----------- | --------- | ----- | -------- | --------- | ---------
p0 | output start time |  -  | no | no | 
p1 | duration |  -  | no | no | 
p2 | amp |  -  | yes | no | 
p3 | fixed table |  -  | yes | yes | 

   The dynamically-controlled values of the p2 'amp' field will print unless
   the optional p3 parameter is present.  See the *Usage Notes* below.

  

-----

  
**DUMP** is a "utility" instrument that can be used to debug PField
variables representing table data or control input streams. If p3 is not
present, the values coming through the "amp" PField (p2) will be
printed. How often printing occurs is determined by the
[control\_rate](../scorefile/control_rate.html) setting in the
scorefile. If p3 is present, all of the values in the table represented
by p3 will print at each [control\_rate](../scorefile/control_rate.html)
interval.

### Usage Notes

This can generate a <u>huge</u> amount of printed data. It is best when
debugging table values (p3) to use small table sizes, since the entire
table is printed at each interval.

### Sample Scores

very basic:

```cpp
   rtsetparams(44100, 1)
   load("DUMP")

   ampenv = maketable("line", 1000, 0,0, 1,1, 2,0)
   DUMP(0, 2, ampenv)
```

  
  
more advanced:

```cpp
   rtsetparams(44100, 1)
   load("DUMP")

   control_rate(100)

   amp = maketable("line", 1000, 0,0, 1,1, 2,0)
   amp = makefilter(amp, "fitrange", -100, 300)

   DUMP(0, dur, amp)
```

  

-----

### See Also

[maketable](../scorefile/maketable.html),
[makemonitor](../scorefile/makemonitor.html),
[dumptable](../scorefile/dumptable.html),
[plottable](../scorefile/plottable.html),
[control\_rate](../scorefile/control_rate.html),
[print](../scorefile/print.html), [printf](../scorefile/printf.html)
