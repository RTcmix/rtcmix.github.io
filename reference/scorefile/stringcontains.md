---
title: stringcontains()
layout: ref
---

## stringcontains

Return true if a string contains a specified substring.

-----

### Synopsis

doesContain = **stringcontains**(*string\_to\_search*, *string\_to\_find*)

-----

### Description

**stringcontains** returns true if the substring is contained within the provided string.  The compare is case-sensitive.

-----

### Arguments

  - *string\_to\_search*  
    The string to search.
    
  - *string\_to\_find*
  	 The substring to search for.

-----

### Examples

```cpp
contains = stringcontains("the haystack", "needle")  // will return false
contains = stringcontains("the haystack", "hay")  // will return true
```

-----

### See Also
[stringtofloat](stringtofloat.html)
