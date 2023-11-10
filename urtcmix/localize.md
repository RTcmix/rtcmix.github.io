---
title: LOCALIZE()
layout: ref
---

# uRTcmix LOCALIZE() Instument

*[rtcmix.org documentation for
LOCALIZE()](http://rtcmix.org/reference/instruments/LOCALIZE.php)*  
*[LOCALIZE() video tutorial](videotutorials.php#localize)*  
  

## initial setup

The LOCALIZE() RTcmix instrument uses a sound 'ray-tracing' approach
combined with a simple HRTF model to position a sound source in virtual
space relative to a listener. In Unity, the 'sound listener' for a given
scene is usually the Main Camera. We will use that as our destination
for sound emanating from a source.

We need to add the C# script [getMyTransform.cs](getMyTransform.cs) to
the Main Camera. This will allow us to get the proper set of transform
coordinates to use with the LOCALIZE() instrument. Add this C# script to
your Assetts and drag it onto the Main Camera.  
  
  

## localize a sound source

Instead of having the LOCALIZE() instrument operate on the Main Camera
object (keeping all the incoming audio streams separate for individual
localization would be very difficult), we will instead apply the
localization delays and amplitude calculations at the sound source. To
do this, we will need to find out from the Main Camera what the sound
source position is relative to the camera so that the correct parameters
can be used. This is what the "getMyTransform.cs" script does.

We declare a class variable to reference the getMyTransform component we
have added to the Main Camera:

We then locate that component on the Main Camera:

Next we connect the output(s) of any sound-generating instruments
attached to our sound source object to the LOCALIZE() instrument, and we
start it running with PFields attached to the source coordinate
parameters (this is usually done in the Start() method):

All we then need to do is to update the 'srcX/srcY/srcZ' PFields from
the location of the sound source, *relative* to the Main Camera sound
listener. We can do this in the Update() method, relying upon the
getTransform() script of the getMyTransform() component we added to the
Main Camera:

The swap of the y and z axes are because of the Unity convention of
using the y-axis as the vertical axis instead of the z axis.

Of course this setup of the LOCALIZE() instrument assumes that the sound
source has been configured to pass through the LOCALIZE() instrument.
This is done by using the rtinput("AUDIO") scorefile command in a chain
of sound-producing game objects, or by using the bus_config() system:

One warning: if a sound source is moving quickly, a rapid clickihg or
'zipper' noise might occur. This is because the source position is being
updated at the slow frame rate, causing visually imperceptible 'jumps'
in the position for each frame rendered. However, these 'jumps' can
introduce discontinuities in the audio signal, resulting in the noise.
This can be minimized or eliminated by using the "smooth" filter type of
the RTcxmix makefilter() command. makefilter() with a "smooth" filter
type interpolates values coming through a PField, acting as a low-pass
filter to smooth out the discontinuities. An example of how this is set
up:

## LOCALIZE() instrument parameters

Parameters with an asterisk(\*) are dynamic PFIelds:

Often a large 'overall amp' (p\[3\]) value will need to be used,
especially if the inverse-square distance calculation (p14 = 2) is used.
This is essentially how sound works in the Real World, but we have no
reflection calculations from nearby surfaces, so the amplitude falls
very rapidly. It's as if everything was happening in an anechoic
chamber. Altering the overall amp multiplier can compensate for this.
Use values that are necessary to achieve the results you want.
