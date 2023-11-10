---
title: uRTcmix Demos
layout: ref
---

# uRTcmix Demos

*November, 2019*  
  
Here are a number of Unity projects showing various ways of using
uRTcmix within Unity. These were created using Unity v2.18, but they
should work in later versions.  
  
  
***spheresound demos***  
  
These were the first demos created for uRTcmix. They are very simple
demonstrations of how the system works. They're called "spheresound"
because the uRTcmix scripts are associated with a sphere game object in
each scene. There is no movement or any visual interaction -- only
audio.  
  

- [spheresound-demo.zip](demos/spheresound-demo.zip) -- essentially the
  same as the "beep" prefab that comes with the uRTcmix.unitypackage as
  a small demo. It plays a 787.8 sine wave for 7 seconds on start-up.  
    
- [spheresound2-demo.zip](demos/spheresound2-demo.zip) -- plays a
  continuous sine wave. The up/down arrow keys add and subtract 10hz to
  the frequency.  
    
- [spheresound3-demo.zip](demos/spheresound3-demo.zip) -- produces a
  small random granular 'burst' on start-up. Shows how to read in an
  RTcmix text scorefile for use in the Unity script.  
    
- [spheresound4-demo.zip](demos/spheresound4-demo.zip) -- reads an
  RTcmix text scorefile and continuously repeats random granular burst
  using the MAXBANG() RTcmix instrument.  
    
- [spheresound5-demo.zip](demos/spheresound5-demo.zip) -- repeats a
  random granular score using MAXBANG(); up/down arrow keys add and
  subtract 10hz to the base frequency, left/right arrow keys set upper
  and lower bounds for the random frequency choice.

  
  
***class demos***  
  
Most all of these come from the 2017 [Sound: Advanced Topics
II](http://sites.music.columbia.edu/cmc/courses/g6611/spring2017/)
graduate seminar at Columbia University (hence the "class\*" name on
many of them). The projects listed below have been updated to reflect
the current uRTcmix package, but the original
[syllabus/week-listing](http://sites.music.columbia.edu/cmc/courses/g6611/spring2017/syl.html)
for the class contains the work-ups done to produce the final projects.
Although they use an older version of uRTcmix, they may be
instructive.  
  

- [simplecat.zip](demos/simplecat.zip) -- very simple Unity playback of
  cat meow sample when a ball hits a rotating cube. Arrow keys move the
  ball. This only uses the basic Unity sample-playback capability; no
  uRTcmix is involved.  
    
- [simplecatAM.zip](demos/simplecatAM.zip) -- cat meow sample is
  processed through an RTcmix AM() script, plus the brightness of a
  repeating STRUM2() RTcmix instrument is determined by the distance to
  a rotating cube. Arrow keys move the ball. This also demonstrates a
  slightly different way of sending the AM score info, using a method on
  the "catAM.cs" script (attached to the rotating cube \["mysticcube"\]
  game object).  
    
- [threetowers.zip](demos/threetowers.zip) -- RTcmix scripts using
  parameters dependent upon the distance from each of three towers, with
  an ambient wind sound in the background ("windsound.cs" script
  attached to the empty "windsound" game object). Two of the tower
  scripts unfold the chaotic 'population equation' (logistic map) with
  the R value (chaos!) getting higher as you approach the tower. One of
  these scripts uses WAVETABLE() and the other uses the STRUM2()
  instrument. The third tower opens a low-pass filter on a low drone
  sound as the distance to the tower decreases. Arrow keys move the
  observer (ball), and the "z" and "x" keys will start rotation left and
  right. The space-bar stops any rotation.  
    
- [classtowers1.zip](demos/classtowers1.zip) -- a cylinder moves up and
  down, with FMINST() parameters being modified by the height. No
  movement, very simple.  
    
- [classtowersall.zip](demos/classtowersall.zip) -- 4 cylinders going up
  and down, like 'classtowers1'. a fifth rectangle in the middle
  periodiocally moves up and down, also modifying FMINST() parameters by
  the height. Arrow keys move around; COMMAND-up/down arrow keys move up
  and down, "w"/"x"/"a"/"d" keys will rotate the 'camera' (stop the
  rotation by pressing the space-bar) without changing the the
  orientation of the underlying observer object. The amplitude of the
  sound of each tower is also modulated by distance, although it isn't a
  strong localization effect.  
    
- [classffly3.zip](demos/classffly3.zip) -- point lights randomly fade
  up.down with associated sound. Each light is localized using the
  RTcmix LOCALIZE() instrument. Arrow keys move the observer;
  COMMAND-up/down will move up or down. "a" and "d" will start rotation
  left or right, with the space-bar stopping the rotation.  
    
- [classwater7.zip](demos/classwater7.zip) -- generate 'droplets'
  (spheres) on-the-fly, falling to hit a simple water-mesh shader model.
  The collisions generate localized (via the LOCALIZE() instrument)
  sound, and the droplets shatter into a particle system. Arrow keys
  move the observer; COMMAND-up/down will move up or down. "a" and "d"
  will start rotation left or right, with the space-bar stopping the
  rotation.  
    
- [classblobj4.zip](demos/classblobj4.zip) -- This is a relativley
  complex demo. The filter cutoff of the low pulsing sound is set by the
  distance to the main object. The main object was created using the
  Blender modeling application. The granular synthesis sounds of the
  four flame-particle systems below the object are each localized with
  the RTcmix LOCALIZE() instrument. These flame-sounds are damped when
  the observer enters the main object, and reverberation is turned up.
  Arrow keys move the observer; COMMAND-up/down will move up or down.
  "a" and "d" will start rotation left or right, with the space-bar
  stopping the rotation. "w" and "x" will rotate the camera (not the
  obeserver) up or down, with the space-bar stopping the rotation. I'm
  not sure why we named this "classblob4j".

  
  
***CCM demos***  
  
These two demos come from a weekend workshop done with Mara Helmuth's
students at the Cincinnati Conservatory of Music. The students had
already done some work with uRTcmix, so these demos cover slightly more
advanced uses.  
  

- [CCM-demo1.zip](demos/CCM-demo1.zip) -- This demo shows how to send
  sound sample data from several separate RTcmix processes into one for
  'global' treatemnt. The 'rvb' game object collects the sound from
  several other RTcmix game object processes and applies the RTcmix
  GVERB() reverberation instrument to the combined signals. Arrow keys
  move the observer; COMMAND-up/down will move up or down. "a" and "d"
  will start rotation left or right, with the space-bar stopping the
  rotation (the sound from the game objects is localized).  
    
- [CCM-demo2.zip](demos/CCM-demo2.zip) -- This is a simple 'trigger'
  demo, and it also instantiates a very basic terrain. Going through the
  thin rectangle in the valley will change the RTcmix output. Turn the
  mesh-renderer off and the trigger will be invisible. Arrow keys move
  the observer; COMMAND-up/down will move up or down. "a" and "d" will
  start rotation left or right, with the space-bar stopping the
  rotation. This also shows how separate RTcmix game object scripts can
  access the same RTcmix process using the same "objno" designator.

  
  
In addition to these examples, the work done in the 2019 [Sound:
Advanced Topics
I](http://sites.music.columbia.edu/cmc/courses/g6610/fall2019/) graduate
seminar should be helpful. Check the weekly links on the [course
syllabus](http://sites.music.columbia.edu/cmc/courses/g6610/fall2019/syl.html)
for examples of developing a virtual environment with various uRTcmix
features.
