---
title: uRTcmix Tutorial
layout: ref
---

# uRTcmix Tutorial

## installing the uRTcmix package

*click* [uRTcmix.unitypackage](uRTcmix.unitypackage) *to download the
uRTcmix package with supporting files*

<span class="small">***Note:** If you are installing \[rtcmix~\] on
macOS machines running Catalina (10.15.x) or later, you will probably
get the Apple 'gatekeeper' message about the library/application not
able to run because it isn't from a 'signed developer'. One of these
days We'll wade through the impenetrable documentation about how to do
this, but for now you can follow the instructions [here](annoying.html)
to get it going.*</span>

To import uRTcmix for use, start a new Unity project. Drag the
*uRTcmix.unitypackage* into the Assets of your project. The resulting
"uRTcmix/" folder will have three subfolders:

  
Drag the *RTcmixmain.prefab* object from your Assets up to the
GameObject-instantiation hierarchy tab (where the "Directional Light"
and "Main Camera" objects are located).  
  
At this point, you will probably get an error when you add the
*RTcmixmain.prefab* object to your object list/hierarchy:

To correct this, from your top menus go to Edit-\>Project
Settings-\>Player and scroll down a bit in your Inspector window. Open
the "Other Settings" tab (if it isn't already). Scroll down and check
the box next to the "Allow 'unsafe' Code" setting.  
  
  
You're now set to use uRTcmix!  
  
  

## make a beep

Drag the *beep.prefab* object from your Assets up to the
GameObject-instantiation hierarchy tab. Hit the 'play' button and you
should hear an 8.7-second sine wave sound with a pitch of
G-above-middle-C.  
  
  
***inside the "beep" object**  
  
  
Open up the "beep.cs" script to see how the "beep" object works. At the
top of the file, you should see this*

Those lines of code allow the C# compiler to access the methods it needs
to run the script in Unity properly. NOTE: If you use any "String"
operations with Unity, you will need to add:

to the above set of "using ..." lines.  
  
  
Next you will see these lines:

The int objno = 0; declares the objno variable and sets it to 0. This is
how we keep track of different instantiations of RTcmix within Unity.
Each RTcmix process we attach to a game object can have a different
objno, keeping it separate from other executing RTcmixes.

The rtcmixmain RTcmix; statement declares a variable that we will use to
access the RTcmix functions from the RTcmixmain object.

The private bool did_start = false; statement declares a boolean
(true/false) variable, did_start; and sets it initially to "false". This
is so that the "beep" script won't try to run audio until RTcmix is
initialized. The objno, RTcmix and did_start variables are declared here
at the top of the class definition, within the "beep" class but outside
any of the "beep" methods because they will be used throught the entire
"beep" class.

Next we add a new method, Awake() and a new line of code to the Start()
method:

The first assignment of the RTcmix variable finds the RTcmixmain game
object and then accesses the "rtcmixmain" script on that object.

Once we have made that connection, we can use the RTcmix variable to
invoke the functions defined in the "rtcmixmain.cs" script. We do this
in the very first line of the Start() method: RTcmix.initRTcmix (objno);
to initialize the RTcmix process for the objno we want to use (in this
case, "0").

We then add a single line to our Start() ethod that will send a very
simple (one line!) script to RTcmix:

*\[note: we are assuming at least a basic grasp of the RTcmix language.
See the [rtcmix.org](http://rtcmix.org) web site for tutorials and
information about RTcmix.\]*

This sends the String (the score) in the double-quotes to RTcmix for
parsing and execution. The final parameter, objno, does what it did in
the RTcmix.initRTcmix (objno) case; it designates which RTcmix process
to use.

Finally, because we have now fully initialized RTcmix, we set did_start
= true; to allow audio processing in this script to take place.

Our finished Start() method then looks like this:

The RTcmix.SendScore(...) will trigger the 'beep'.

But you will need to add two additional methods to the "beep" class for
this to work:

The initial if (!did_start) return; line will prevent audio processing
from occuring if RTcmix has not been initialized yet.

The OnAudioFilterRead() method passes audio data to and from the "beep"
class through the data array parameter. We hand that off to RTcmix with
the RTcmix.runRTcmix (data, objno, 0); line, invoking the runRTcmix()
method from the "rtcmixmain.cs" file. runRTcmix() will fill the data
array with one buffers-worth of sound samples from the RTcmix process
(usually 512 samples in Unity). Note the use of objno again.

The final "0" after the objno variable signals that we are only
synthesizing sound, not processing any sound that comes into the 'beep'
class from a previous sound-generating object in Unity. A final "1" in
the RTcmix.runRTcmix() would allow us to do input signal-processing. We
will discuss this later.

The OnApplicationQuit() method resets RTcmix by destroying the
particular instantiation and setting did_start to "false" so that the
next time we run the scene (and the Start() method is called),
everything will be set up properly.

Our final "beep.cs" file looks like this:

  
  

## beeping...

To make a repeating beep pattern is very simple in RTcmix. We can set
RTcmix to repeat the score indefinitely using the ability of RTcmix to
generate a 'bang' to re-trigger the score. (A 'bang' is a notion that
comes from Max/MSP and the **\[rtcmix~\]** object; it is used to trigger
various operations in Max.) For this we will use the MAXBANG() RTcmix
instrument. MAXBANG() takes a single parameter -- how many seconds from
"now" do we generate a 'bang'.

This requires only two alterations in our original "beep" script. One is
to the initial RTcmix.SendScore() score that we send to include a
MAXBANG() scheduled 'bang' in the future, and one to detect when that
'bang' occurs and respond appropriately.

Our revised Start() method looks like this:

Note that we have reduced the duration of the WAVETABLE() to 0.2
seconds. This is because we are scheduling a 'bang' at 0.3 seconds after
the score is sent: MAXBANG(0.3). We will have 0.1 seconds of silence
between the stop of that first WAVETABLE() note and the interception of
the 'bang'.

We will detect that MAXBANG()-scheduled 'bang' within the
OnAudioFilterRead() method. We accomplish this by using the uRTcmix
function RTcmix.checkbangRTcmix(). This function will return "1" when a
'bang' is detected at a scheduled time. The OnAudioFilterRead() method
now looks like this:

By sending a MAXBANG() again when we generate the next WAVETABLE() beep,
we will then intercept another 'bang' in the future, and the
retriggering will continue indefinitely.  
  
<span id="reading_rtcmix_scorefiles"></span>

## reading RTcmix scorefiles

Let's suppose we've developed a more complex RTcmix score that
instantiates a simple granular synthesis process:

If we wanted to send this using our 'imbedded string' approach we've
been employing with RTcmix.SendScore(), the code would look something
like this:

which is pretty ugly and fairly difficult to modify/debug. We can make
things a little easier by using the [scoralyzer](scoralyzer.html)
command to pre-format our RTcmix script into a valid C# string variable,
but it would still be annoying to edit. There is a better way: if we
save the original RTcmix code for the granular process into a text file,
perhaps "granular.txt", we can add it as an asset to our Unity project
and load, read, use, and edit the file from within our Unity development
environment. NOTE: These files have to have the ".txt" extension for
Unity to recognize them as valid text files.

If we add the file "granular.txt" containing the RTcmix code above to
our Unity assets, we can edit it the same way we edit C# scripts that
work as assets in Unity. To access the RTcmix code in the file in our
game object script, you can take advantage of the Unity/C# "TextAsset"
class.

Essentially, you declare a public variable that will 'hold' your
TextAsset scorefile, and you can then extract a string from that
TextAsset for use by the RTcmix.SendScore() method. Here is how to set
it up:

After this setup, the granscore variable (which is a 'global' variable
in the class because -- like with the int objno = 0; etc. global
variables -- we declared it at the top of the class definition, outside
any class methods) is the RTcmix scorefile string that you can send for
parsing/execution in the appropriate place in your code:

The only trickiness is to recognize that by declaring public TextAsset
grantextfile; at the top of the class definition, it exposes a slot in
the Unity inspector of the associated game object for that grantextfile
variable. You need to drag the "granular.txt" file from your Assets into
the slot so that the public grantextfile variable will reference the
"granular.txt" file with the RTcmix code in it.  
  

## alter a beep

RTcmix has two different ways for allowing data from Unity to change the
parameters of a sound being synthesized or processed. One way is to
change parameters of an executing RTcmix instrument dynamically while it
is making sound. RTcmix has 'special' parameters for all of it's
synthesis/signal-processing instruments called "PFields" that allow you
to do this. These can be addressed from within Unity. We will alter the
non-repeatig 'beep' script to show this.

In the 'beep' class, we will use another variable, frequency, declared
globally (at the top of the class definition) to set the frequency of
the repeating WAVETABLE() RTcmix instrument. Our setup now looks like
this:

We set an initial value (700.0f) for the frequency variable in our
Start() method. It corresponds to the "700.0" that was set as the
default value in the RTcmix makeconnection() scorefile command. We're
setting frequency as a 'float' variable, but it could be cast as a
'double' or an 'int'. RTcmix will accept them all.

The WAVETABLE() is started with a duration of "99999" -- this is just to
insure that the note continues to play so that we may alter the
frequency when we want. That value is arbitrary, and if 99999 seconds is
too short it can be extended.

Next we add script code to the "beep" class definition to alter the
value of the frequency variable. We do this in our Update() method by
seeing if the user has typed an Up or Down arrow:

If the Up arrow is pushed, 10.0 will be added to the value of frequency.
The Down arrow will subtract 10.0 Hz. The modified value is then sent to
the executing WAVETABLE() instrument via the RTcmix.setpfieldRTcmix (1,
frequency, objno); method. The "1" is the inlet number, set by the
makeconnection() command in the RTcmix score.

This will work, but it's not the most efficient way of doing the task.
The Update() method will be called at the frame rate of Unity, usually
60 times/second. We only need to update our WAVETABLE() frequency value
when the user presses one of the arrow keys -- probably not 60
times/second. A better approach would be this:

which would only call the RTcmix.setpfieldRTcmix() method when a key was
pressed.  
  

## modifying RTcmix scorefiles

RTcmix scores themselves can also be altered dynamically to open up
another possibility for interactively sending data from Unity into
RTcmix. Sscorefile variable values can be reassigned before the score is
sent to RTcmix. The "rtcmixmain.cs" file contains a utility function to
accomplish this.

To take advantage of this capability, the RTcmix score needs to include
special variables with a"\$" as the first character and a number as the
second (this is also from the **\[rtcmix~\]** object in Max/MSP). The
"\$" signals that this variable needs to have a value assigned from
"outside" RTcmix, and the number indicates the ordering of the
assignments.

For example, the small RTcmix score:

will expect that the value for the amplitude parameter (\$1) and the
frequency parameter (\$2) will be set from Unity values "outside"
RTcmix. The utility function setscorevalsRTcmix() included in the
"rtcmixmain.cs" file does this by taking a score string with the
\$variables embedded within and returning a score string with
substitutions made.

Let's look at a modified version of the RTcmix code in the
"granular.txt" example from above to show how these \$variables work:

The values for the hifreq and lowfreq variables can be set using the
setscorevalsRTcmix() utility function. This function takes a score
string with \$variables and substitutes them with values from a list of
parameters passed with the function. It then returns a string with the
values set for RTcmix to parse and execute.

If we took the granular synthesis score above with the \$variables set
for hifreq and lowfreq and put it in our "granular.txt" file, we can do
this substitution with the following code:

This \$variable substitution gives us another opening for transferring
values from Unity into RTcmix. If we use the recurring MAXBANG() scheme
for re-triggering the score, we can set new values for the \$variables
every time the score is passed into RTcmix. The following code shows how
to do this, using the Up and Down arrow keys to modify the value sent
for \$1 (hifreq) and the Right and Left arrow keys setting the value for
\$2 (lowfreq):

Be aware that depending on how often a new score is sent to RTcmix, a
latency between setting the values in Unity and having them take effect
in RTcmix will occur.  
  

## *links*

[Sound: Advanced Topics I (Fall
2019)](http://sites.music.columbia.edu/cmc/courses/g6610/fall2019/syl.html)  
Each week of this syllabus links to class projects and resources showing
various uses of Unity and RTcmix.  
  

[Sound: Advanced Topics I (Fall
2017)](http://sites.music.columbia.edu/cmc/courses/g6610/fall2017/syl.html)  
An older version of the previous link, with somewhat simpler projects.
These use an older version of uRTcmix, but the projects should still run
and the general demonstrations might be useful.  
  

[RTcmix.org](http://rtcmix.org)  
The main RTcmix page. Tutorials, code, useful information.
