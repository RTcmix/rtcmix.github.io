---
title: inrepos()
layout: ref
---

### inrepos/outrepos

*older disk-based cmix instruments*

**inrepos** is superceded by [rtinrepos](rtinrepos.html). There is no
equivalent for **outrepos** in RTcmix because of the way the scheduler
works, The alternative is to use the scheduler to set the timepoint for
output-writing.  
  
From the source code:

-----

int inrepos(int samps, int fno)  
  
samps = number of samples to move forwards or backwards (negative) in time  
fno = open soundfile number

int outrepos(int samps, int fno)  
  
samps = number of samples to move forwards or backwards (negative) in time  
fno = open soundfile number

Both functions return a pointer to the file being operated upon, or exit
on failure.
