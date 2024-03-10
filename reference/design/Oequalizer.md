---
title: Oequalizer()
layout: ref
---

### Oequalizer

*INSTRUMENT design -- multi-purpose equalizer (filter) object*  
  
The **Oequalizer** object uses a [biquad
filter](https://peabody.sapp.org/class/350.838/lab/mybiquad/) algorithm
to instantiate a number of different filter types (low-pass, high-pass,
band-pass, etc.). The code was based on work done by [Tom St.
Denis](https://tomstdenis.home.dhs.org/), using formulas for the filter
coeeficients from Robert Bristow-Johnson's on-line document [The
Audio-EQ-Cookbook](https://www.musicdsp.org/files/Audio-EQ-Cookbook.txt).

The older functions [reson](reson.html) and [resonz](resonz.html) do
similar signal-processing actions, but using different and somewhat less
flexible algorithms. For very simple filtering, the
[Oonepole](Oonepole.html) object may be more appropriate.

-----

### Constructors

**Oequalizer(***float SR, OeqType filter\_type***)**  
  

<u>*SR*</u> is the current sampling rate (an
[Instrument](Instrument.html) class variable).  
<u>*filter\_type*</u> is the kind of filter that will be implemented by
the biquad equation. These types are defined in the *OeqType* structure
found in the *RTcmix/genlib/Oequalizer.h* file:

  - *OeqLowPass* -- low-pass filter  
  - *OeqHighPass* -- high-pass filter  
  - *OeqBandPassCSG* -- band-pass filter with "constant skirt gain", the
    peak gain will be related to the "Q" of the filter  
  - *OeqBandPassCPG/OeqBandPass* -- band-pass filter with "constant peak
    gain", the peak gain will be 0 dB  
  - *OeqNotch* -- notch filter  
  - *OeqAllPass* -- allpass filter  
  - *OeqPeaking* -- peaking filter (good for creating "quacking" sounds;
    used in... oh, you figure it out :-))  
  - *OeqLowShelf* -- low shelf filter  
  - *OeqHighShelf* -- high shelf filter

  
  

-----

### Access Methods

  

-----

### Examples

  

-----

### See Also
