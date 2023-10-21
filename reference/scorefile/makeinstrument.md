---
title: makeinstrument()
layout: ref
---

## makeinstrument

Create a handle for the CHAIN instrument.

-----

### Synopsis

inst\_handle = **makeinstrument**(*"INSTRUMENT\_NAME", inst\_arg0,
inst\_arg1, ...*);

-----

### Description

**makeinstrument** is used to create handles for instruments that will
become the arguments to the [CHAIN](../instruments/CHAIN.html)
instrument. These are functionally identical to instruments created in
the normal fashion.

-----

### Arguments

  - *INSTRUMENT\_NAME*  
    The name of the instrument to be created. This will always match the
    name that would have been used in a normal instrument call.

  - *inst\_arg0, inst\_arg1, ...*  
    The first, second, etc., arguments that would have been handed to
    the instrument in a normal call.

-----

### Examples

Example: Create a TRANS instrument handle:

```cpp
    outskip = 0;
    inskip = 0;
    dur = 2;
    amp = 100;
    transposition = -0.09;

    trans = makeinstrument("TRANS", outskip, inskip, dur, amp, transposition);
```

-----

### See Also

[CHAIN](../instruments/CHAIN.html)
