---
title: plottable()
layout: ref
---

## plottable

Generate a graphics plot of the contents of a table
given an associated table-handle variable.

-----

### Synopsis

**plottable**(*table\_handle*\[, *pause*\]\[, *"gnuplot\_cmd1"*,
*"gnuplot\_cmd2"*, ... \])

Parameters inside the \[brackets\] are optional.

-----

### Description

**plottable** is a useful utility that will create a graphics plot of a
table associated with a particular table-handle. The plot appears in a
separate window, and uses the public-domain
[gnuplot](http://gnuplot.sourceforge.net/) program. MacOS users
will also need to have the public-domain
[aquaterm](http://sourceforge.net/projects/aquaterm/) package installed
for *gnuplot* to run correctly.

-----

### Arguments

  - *table\_handle*  
      
    The table-handle identifier for the table to be plotted.

  - <span id="item_pause">*pause*</span>  
      
    The length of time (in seconds) to display the plot before
    proceeding with scorefile processing. The default is 10 seconds.

  - <span id="item_gnuplot_cmd">*gnuplot\_cmd1, ... gnuplot\_cmdN*</span>  
      
    These optional string arguments are plotting commands that *gnuplot*
    may use to modify the generated graph. See the [gnuplot
    documentation](http://gnuplot.sourceforge.net/documentation.html)
    for more information about these commands. The default is *"with
    lines"*.
    
    If something goes wrong with the gnuplot syntax, there'll be some
    leftover files in the /tmp directory, with names like
    "rtcmix\_plot\_data\_xAb5x8."

-----

### Examples

```cpp
   table = maketable("wave", 1000, 1, 0.1, 0.3)
   plottable(table, 30)
```

The above score will produce the following plot in a separate window (it
will appear for 30 seconds on non-macOS systems):  
  

![](images/plottable1.png)

  
  
This scorefile:

```cpp
   table = maketable("wave", 1000, 1, 0.1, 0.3)
   plottable(table, 15, "with points")
```

will produce this plot:  
  

![](images/plottable2.png)

  
  
and display it for 15 seconds (on non-macOS systems). The following
scorefile:

```cpp
   table = maketable("wave", 1000, 1, 0.1, 0.3)
   plottable(table, "with impulses")
```

generates this kind of plot:  
  

![](images/plottable3.png)

  
  

-----

### See Also

[dumptable](dumptable.html), [maketable](maketable.html),
[makeconverter](makeconverter.html), [makefilter](makefilter.html),
[modtable](modtable.html), [tablelen](tablelen.html),
[samptable](samptable.html)
