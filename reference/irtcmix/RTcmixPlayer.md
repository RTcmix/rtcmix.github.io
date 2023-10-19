---
title: iRTcmix Class Reference - RTcmixPlayer
layout: ref
---

# iRTcmix Class Reference - RTcmixPlayer

*\[Note: This page is meant to serve as a reference, not as a teaching
tool. If you are new to iRTcmix you should start with the [demos on the
iOS page](../../irtcmix/index.html) and the
[tutorials](../../tutorials/index.html).\]*

  
The *RTcmixPlayer* class acts as the interface to the RTcmix 'engine'
that drives audio in iOS. It also handles the RTcmix-parsing of score
texts with the concommitant scheduling and realization of audio events
designated in RTcmix scores.

## RTcmixPlayer Properties

<span id="sampleRate"></span>

#### CGFloat sampleRate

The RTcmixPlayer variable holding the audio sampling rate (the default
is 44100.0). <span id="vectorSize"></span>

#### NSInteger vectorSize

The size of the buffers used by RTcmix. This should be a power of 2 (the
default is 512). Smaller values decrease responsive latency, while
larger values allow for more efficient processing (i.e. you can do more)
but increase responsive latency. <span id="vectorSize"></span>

#### NSInteger numberOfChannels

The number of channels in the audio stream (the default is 2).
<span id="audioInputFlag"></span>

#### BOOL audioInputFlag

"YES" if audio input enabled, "NO" if output only.
<span id="audioInputFlag"></span>

#### BOOL shouldRunInBackground

"YES" if RTcmix is allowed to generate audio in the background, "NO" if
not. <span id="volume"></span>

#### CGFloat volume

The multiplier used to scale the RTcmix output volume (usually 0.0 -
1.0; the default is 1.0). (Warning: This is likely to be deprecated
soon. You should use the *setVolumeLevel:* method instead of changing
the *volume* property directly.) <span id="scoreDict"></span>

#### NSMutableDictionary \*scoreDict

A pointer to the dictionary used for storing and access RTcmix score
objects.

## RTcmixPlayer Methods

### Initialization and Setup

<span id="scoreDict"></span>

#### \+ (id)sharedManager

This is the method used to instantiate a new RTcmixPlayer object as a
singleton for use in an iRTcmix application. This will guarantee that
only one instance of the RTcmix engine and concomitant audio processing
will be running throughout all coded objects in the app.
<span id="scoreDict"></span>

#### \- (void)startAudio

This is used to start the RTcmix engine and audio conversion. If not
set, the method will assign default values for these variables:

``` 
    self.volume = 1.0;
    self.numberOfChannels = 2;
    self.sampleRate = 44100.0;
    self.vectorSize = 512;
```

<span id="scoreDict"></span>

#### \- (void)resetAudio

Used to halt current RTcmix processing and restart audio processing.

### Audio Utility

<span id="scoreDict"></span>

#### \- (void)pauseAudio

Temporarily halts the RemoteIO Audio Unit and RTcmix. Scheduled events
and audio will not be removed from the conversion queue, and will be
active if audio conversion is re-established. This is used for an
incoming pre-emptive application (like a phone call).
<span id="scoreDict"></span>

#### \- (void)resumeAudio

Restarts audio after the -pauseAudio method has been called. All prior
property values and parameters are retained from the previously active
Audio Session. <span id="scoreDict"></span>

#### \- (void)setVolumeLevel:(float)newvolume

Method to set the RTcmix multiplier volume for audio output (usually 0.0
- 1.0).

param:

  - (float)newvolume -- a floating-point number for the multiplier value

### Data and Communication

<span id="scoreDict"></span>

#### \- (int)setSampleBuffer:(NSString \*)bufferName withSoundFile:(NSString \*)soundFilePath

This will load samples from an existing soundfile for access by RTcmix
(using the rtinput("MMBUF", "name") score function) to use in input
signal-processing operations. The buffer memory for the soundfile is
allocated in this method.

note: at present, only loading of AIFF-type soundfiles into an internal
buffer is supported. params:

  - (NSString \*)bufferName -- the 'name' of the buffer, used in the
    RTcmix score - rtinput("MMBUF", "name") - to reference a buffer
    containing sound samples
  - (NSString \*)soundFilePath -- the path to the soundfile to be loaded
    into the buffer.

<span id="scoreDict"></span>

#### \- (void)setInlet:(int)inlet withValue:(Float32)value

This creates an 'inlet' connection to RTcmix via the
makeconnection("inlet", ...) score function. This allows dynamic
updating of PFields in RTcmix Instruments.

params:

  - (int) inlet -- an inlet index nunber set in the RTcmix score
  - (Float32) value -- the initial value of the inlet

### Score Methods

<span id="scoreDict"></span>

#### \- (void)addScore:(NSString \*)name withString:(NSString \*)score

This method takes a the text of a Minc score and stores it as an
RTcmixScore object in the RTcmixScore 'dictionary'. This allows for easy
retrieval of this score during sound processing and synthesis. The
"name" parameter is used as the key for retrieval from the RTcmixScore
dictionary.

params:

  - (NSString \*)name -- key name used to reference the score
  - (NSString \*)score -- text (string) of a valid RTcmix (Minc) score

<span id="scoreDict"></span>

#### \- (void)parseScoreWithRTcmixScore:(RTcmixScore \*)score

This method takes a valid RTcmixScore (see the [documentation for the
RTcmixScore class](RTcmixScore.html)) and calls the RTcmix
parse\_score() function to parse and schedule (using the RTcmix Minc
parser) the instruments and notes of the score.

params:

  - (RTcmixScore \*)score -- a valid RTcmixScore object reference

<span id="scoreDict"></span>

#### \- (void)parseScoreWithNSString:(NSString \*)score

This method takes a the text of a Minc score (as an NSString) and calls
the RTcmix parse\_score() function to parse and schedule the instruments
and notes of the score.

params:

  - (NSString \*)score -- text (string) of a valid RTcmix (Minc) score

<span id="scoreDict"></span>

#### \- (void)flushAllScripts

This calls the RTcmix flush\_sched() function to remove all scheduled
events from the RTcmix queue. The RTcmix engine continues to run after
this is invoked, however. All symbols and allocated memory by the RTcmix
Minc parser remain active.

### Delegate Methods

Three methods are designated as 'delegate' methods for the RTcmixPlayer
object. This means that they are intended to be coded (if needed) by in
the implementing application in the class chosen as the
*RTcmixPlayerDelegate*. These methods are how you may receive
communicate back to your application from RTcmix.
<span id="scoreDict"></span>

#### \- (void)maxBang

The method called when the RTcmix MAXBANG() instrument generates
'bangs.' <span id="scoreDict"></span>

#### \- (void)maxMessage:(NSArray \*)message

The method called when the RTcmix MAXMESSAGE() instrument generates
messages to return data from RTcmix.

param:

  - (NSArray \*)message - the array returned by RTcmix

<span id="scoreDict"></span>

#### \- (void)maxError:(NSString \*)error

This method allows RTcmix reporting and error messages to be displayed
(see the RTcmix documentation for the
[print\_on()](../scorefile/print_on.html) scorefile command).

param:

  - (NSArray \*)error - the message returned by RTcmix

## Basic Implementation

To access the functionality of the RTcmixPlayer class, you might define
an RTcmixPlayer property of your ViewController called \*rtcmixManager:

``` 
    #import "RTcmixPlayer.h"

    @interface myViewController : UIViewController

    @property (nonatomic, strong) RTcmixPlayer *rtcmixManager;
```

Then the underlying RTcmix engine could be instantiated and started with
the following code:

``` 
    self.rtcmixManager = [RTcmixPlayer sharedManager];
    [self.rtcmixManager startAudio];
```

Typically this code would be located in the -viewDidLoad method of your
ViewController; it will be called when the application is loaded on the
iDevice.

Note the initialization of the RTcmixManager property
(self.rtcmixManager) using the sharedManager method. The RTcmixManager
is being used as a 'singleton' object, with the RTcmixPlayer class
defining a set of 'singleton' methods for this kind of use. That way a
number of different ViewControllers (for more complex applications) all
have access to the same underlying audio engine.

Next you will need a score to send to RTcmix for parsing. The simplest
way is to send an NSString to the *parseScoreWithNSString:* method as in
this example with a one line RTcmix score:

``` 
    [self.rtcmixManager parseScoreWithNSString:@"WAVETABLE(0, 2, 24000, 262.626)"]
```

You may also use the [RTcmixScore class](RTcmixScore.html). First,
create a RTcmixScore object for storage in RTcmixPlayer:

``` 
    NSString *myScorePath = [[NSBundle mainBundle] pathForResource:@"MyScoreFile" ofType:@"sco"];
    NSString *myScoreText = [NSString stringWithContentsOfFile:mainScorePath encoding:NSUTF8StringEncoding error:nil];
    [self.rtcmixManager addScore:@"MyScore" withString:myScoreText];
```

Then use the *parseScoreWithRTcmixScore:* method to send the score to
RTcmix:

``` 
      [self.rtcmixManager parseScoreWithRTcmixScore:helloScore];
```

There are a number of other methods for connecting interface elements
with the RTcmix audio model, including -setInlet, -maxBang,
-setSampleBuffer, etc. These methods reflect the structure of the
max/msp rtcmix\~ object for making the interface-to-RTcmix connection.
This is also why several of the methods are named "max"-something -- it
has nothing to do with the maximum value of a parameter. See the [demo
projects](../../irtcmix/index.html) for examples of how these methods
are used.

The RTcmixPlayer class has methods to control audio playback in certain
situations (-pauseAudio, -resumeAudio, -setVolumeLevel, etc.). Along
with these methods are instance variables that cab set or report the
characteristics of the audio being produced (sampleRate, vectorSize,
numberOfChannels, etc.). The RTcmixPlayer also instantiates the
low-level callbacks necessary for audio production
(-rtcmixPerformCallback) and for servicing interruptions and
property-value changes (-interruptionListenerCallback,
-propListenerCallback).
