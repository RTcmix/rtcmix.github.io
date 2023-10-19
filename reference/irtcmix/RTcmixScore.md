---
title: iRTcmix Class Reference - RTcmixScore
layout: ref
---

# iRTcmix Class Reference - RTcmixScore

*\[Note: This page is meant to serve as a reference, not as a teaching
tool. If you are new to iRTcmix you should start with the [demos on the
iOS page](../../irtcmix/index.html) and the
[tutorials](../../tutorials/index.html).\]*

  
The *RTcmixScore* class is a utility class used to store Minc-based
RTcmix scores (see the [RTcmix
documentation](../../tutorials/standalone.html)) and send them for
parsing/scheduling to the RTcmix 'engine' using the *RTcmixPlayer*
method *parseScoreWithRTcmixScore:*. Using this method allows you to
have variable values that are managed in your iOS app. These stored
values will then be updated in the score prior to being sent to the Minc
parser.

## RTcmixScore Properties

<span id="mainScoreParameters"></span>

#### NSMutableDictionary \*parameterValues

Stores the values that will replace the "%\#" tokens before sending the
main character score buffer to the RTcmix parse\_score() function.
<span id="setupScoreParameters"></span>

#### NSMutableDictionary \*parameterInlets

Stores the inlet number corresponding to any stored "%\#" pfield
variable.

## RTcmixScore Methods

<span id="initWithScore"></span>

#### \- (id)initWithScoreFile:(NSString \*)score

Initializes an RTcmixScore object from a file.

params:

  - (NSString \*)score -- An NSString containing the pathname to the
    RTcmix scorefile.

<span id="initWithNSString"></span>

#### \- (id)initWithNSString:(NSString \*)score

Initializes an RTcmixScore object from a string.

params:

  - (NSString \*)score -- an NSString containing the text of the RTcmix
    score.

## Replacing Variables with %\#

Of special note is the "%\#" variable used in iOS RTcmix scores. This
variable is similar to the "$N" ($1, $2, ...) variables used to bring
external values into the score prior to RTcmix parsing in the Max/MSP
[*irtcmix\~*](../../rtcmix_/index.html) object. *RTcmixScore* stores
values in an NSMutableDictionary to be substituted in place of the "%\#"
tokens in the score before the RTcmix parse\_score() function is called.
Note that if you are not using this feature (i.e. you are sending static
scores that are not programatically changed by your app), it will
generally make more sense to store your score in an NSString and send to
the parser using the *RTcmixPlayer* method *parseScoreWithNSString:*
method.

Suppose we have a file in our bundle called "MyRTcmixScore.sco"
contaning the following lines:

``` 
    beepFrequency = %#
    beepDuration = %#
    beepAmplitude = %#
    
    WAVETABLE(0, beepDuration, beepAmplitude, beepFrequency)
```

We find the path to the scorefile:

``` 
    NSString *scorefilepath = [[NSBundle mainBundle] pathForResource:@"MyRTcmixScore" ofType:@"sco"];
```

We then create an RTcmixScore object:

``` 
    RTcmixScore *myscore = [[RTcmixScore alloc] initWithScore: scorefilepath];
```

Now we have an *RTcmixScore* object named *myscore* that holds the text
of the score as well as placeholder objects for any variable that has a
value of "%\#" in an NSMutableDictionary keyed to the variable names.
Before sending the score to RTcmixPlayer for parsing, we must assign
values to the variables. Since NSDictionary stores objects (i.e. not
ints, floats, etc.), you must store any numbers in an NSNumber.
NSStrings may also be used.

``` 
    NSNumber *beepFrequencyObject = [NSNumber numberWithFloat:262.626];
    NSNumber *beepDurationObject = [NSNumber numberWithFloat:2.5];
    NSNumber *beepAmplitudeObject = [NSNumber numberWithInt:26000];
```

These objects can now then be sent to the RTcmixScore object *myscore*
for storage in the *parameterValues* NSMutableDictionary. The dictionary
keys are the variable names given in the RTcmix score.

``` 
    [myscore.parameterValues setObject:beepFrequencyObject forKey:@"beepFrequency"];
    [myscore.parameterValues setObject:beepDurationObject forKey:@"beepDuration"];
    [myscore.parameterValues setObject:beepAmplitudeObject forKey:@"beepAmplitude"];
```

Now, when you send *myscore* to [RTcmixPlayer](RTcmixPlayer.html)
through the
[-parseScoreWithRTcmixScore:](RTcmixPlayer-2.html#parseScoreWithRTcmixScore)
method it will insert the proper values for all stored variables. After
substitution from the elements in *parameterValues*, the scorefile that
will be sent to the Minc parser will look like this:

``` 
    beepFrequency = 262.626
    beepDuration = 2.5
    beepAmplitude = 26000
    
    WAVETABLE(0, beepDuration, beepAmplitude, beepFrequency)
```

## Using %\# Replacement with PField Variables

PField variables are dynamic variables that may change value during
score execution. See [A Short Tour of PField
Capabilities](../../tutorials/pfields.html) for more a more detailed
discusion. RTcmixScore is also able to store and modify these values.
Starting with a similar score:

``` 
    beepFrequency = makeconnection("inlet", 1, %#) 
    
    WAVETABLE(0, 1000, 26000, beepFrequency)
```

First we find the path to the score file in the application bundle and
initialize our score object (as we did above):

``` 
    NSString *scorefilepath = [[NSBundle mainBundle] pathForResource:@"MyRTcmixScore" ofType:@"sco"];
    RTcmixScore *myscore = [[RTcmixScore alloc] initWithScore: scorefilepath];
```

Now we may set the value of stored variables prior to parsing (again, as
above):

``` 
    NSNumber *beepFrequencyObject = [NSNumber numberWithFloat:262.626];
    [myscore.parameterValues setObject:beepFrequencyObject forKey:@"beepFrequency"];
```

After substitution from the elements in *parameterValues*, the scorefile
that will be sent to the Minc parser will look like this:

``` 
    beepFrequency = makeconnection("inlet", 1, 262.626) 
    
    WAVETABLE(0, 1000, 26000, beepFrequency)
```

Since PField variables may be updated dynamically, we can send a message
to change the value while the script is running. This is done by sending
a message do the "inlet" of the variable. The inlet number corresponding
to a PField variable is stored in RTcmixScore so you don't have to keep
track of it.

``` 
    NSInteger myFreqInlet = [[self.parameterInlets objectForKey:@"beepFrequency"] integerValue];
```

We can send a new value to the running score using the RTcmixPlayer
*setInlet:withValue:* method:

``` 
    [self.myrtcmixPlayer setInlet:myFreqInlet withValue:440.0];
```
