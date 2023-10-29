---
title: error()
layout: ref
---

## error

Report an error condition in a score and generate an error condition.

-----

### Synopsis

**error**("formatted\_error\_message")

-----

### Description

**error** prints its error message and generates an error condition in the system.
This happens immediately upon the parser executing the **error** command.  At that point, all scheduled events are terminated and the parse is exited.  Note: If the option
*exit\_on\_error* is set to 1, **error** will terminate the process.

-----
### Example

```cpp
// Report an error if a requested soundfile does not have a 44100 sampling rate

if (filesr(soundfile_to_open) != 44100)
{
	error("Requested soundfile (%s) sampling rate is %f, not 44100\n",
	      soundfile_to_open, filesr(soundfile_to_open));
}
// else go on and open it
```

-----

### See Also

[system](system.html), [set\_option](set_option.html), [exit](exit.html)
