---
title: exit()
layout: ref
---

## exit

Terminate RTcmix.

-----

### Synopsis

**exit**([error_message])

-----

### Description

**exit** prints its error message, if provided, then closes all open
soundfiles and input streams (if any) and terminates RTcmix execution.
This happens immediately upon the parser encountering the **exit** command
&mdash; all scheduled events are terminated.  Note: If the option
*exit\_on\_error* is set to 0, **exit**
does nothing.

-----

### See Also

[system](system.html), [set\_option](set_option.html)
