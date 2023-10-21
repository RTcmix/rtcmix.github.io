---
title: include()
layout: ref
---

## include

Embed one RTcmix scorefile within another.

-----

### Synopsis

**include** *filename*

-----

### Description

**include** allows you to "include" an external file in another
scorefile. The "included" file will behave as if the text of the file
were part of the original scorefile; i.e. all variable assignments,
table-creation or option-setting done in the "included" portion will
have affect throughout the rest of the 'parent' scorefile. In other
words, the "included" file will behave as if it were typed into the
"parent" scorefile at the point where the **include** directive is
placed.

The syntax is easy, although it differs slightly from the standard
[Minc](Minc.html) or other parser-interface syntax:

```cpp
   include otherfile.sco
```

where *otherfile.sco* can be an absolute or relative path to a scorefile
or scorefile fragment to be included. An **include** statement may be
placed anywhere in the 'parent' scorefile. "included" scorefiles may
also contain **include** statements, up to a nesting depth of 10.

This command makes it convenient to collect a set of values or
parameters in a single file that can then be included in multiple
scorefiles.

The **include** statement <u>has to start in column 1</u>.

This is similar to the *\#include* directive in C/C++.

-----

### Arguments

  - *filename*  
    An absolute or relative pathname to a scorefile or scorefile
    fragment. If your installed **RTcmix** package has an *minclude* directory,
    any scorefile in that directory may be accessed without need of a full path.
    At present the included filename cannot contain spaces or
    other white-space characters (quotes around *filename* will not
    work).

-----

### Examples

```cpp
   dice = random()

   if (dice > 0.5) {
   include firstbigchunk.sco
   } else {
   include secondbigchunk.sco
   }
```

The above scorefile will be altered by the inclusion of
*firstbigchunk.sco* or *secondbigchunk.sco* depending on the random
value of *dice*. Note that the **include** directive is placed in column
1, not indented.

