---
title: Offt()
layout: ref
---

### Offt

*INSTRUMENT design -- Fast Fourier Transform (FFT) object*  
  
**Offt** is a utility object that encapsulates several Fast Fourier
Transform (FFT) operations for use by RTcmix instruments. These include
the allocation of a buffer for the transformation of samples as well as
functions to perform a forward- (real-to-complex) or backward-
(complex-to-real; inverse) transform on the data in the buffer.

Using the **Offt** object requires a little basic knowledge of how the
Fast Fourier Transform operates. Julius O. Smith has a [good math
tutorial](http://ccrma.stanford.edu/~jos/mdft/Fast_Fourier_Transform_FFT.php#18450)
about the FFT, and the [FFTW page](http://www.fftw.org/) is probably the
best source of FFT information currently on the web.

The specific FFT code used by the **Offt** object depends upon how
RTcmix was compiled. The default compilation uses older code written by
Laurent de Soras (available at [musicdsp.org](http://musicdsp.org/) and
other places). If RTcmix was compiled using the "--with-fftw" flag
during configuration, then the [FFTW v. 3](http://www.fftw.org/) library
is used. *\[note: the FFTW library needs to be compiled with the
"--enable-float" flag to work in this object.\]*

-----

### Constructors

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
