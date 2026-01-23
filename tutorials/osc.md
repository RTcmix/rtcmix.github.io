---
title: Using RTcmix as an OSC server
layout: ref
---

# Using RTcmix as an OSC server

The default RTcmix application (invoked by the
[CMIX](../reference/standalone/CMIX.html) command) reads from Minc score files, either via [stdin](https://pubs.opengroup.org/onlinepubs/007904975/functions/stdin.html) or passed-in via the -f option. If
you configure the build of RTcmix with support for [Open Sound Control](https://ccrma.stanford.edu/groups/osc/index.html) (OSC) using the
*--with-osc* flag (see the [installation
guide](../referene/standalone/index.html) for information about this), then you can run RTcmix as an OSC server by using the -o option (see the full set of options [here](../reference/standalone/CMIX.html)).

## Communicating with the CMIX OSC server

OSC servers are messaged via APIs which supports the OSC specification.  In the current version of RTcmix, we use APIs in the [**liblo**](https://liblo.sourceforge.net) libraries to both receive messages for the server and to send messages via the client-side utility **osc_send** which is included with the distribution.  For details on how to write your own client using **liblo** or another OSC library, consult the OSC documentation [here](https://ccrma.stanford.edu/groups/osc/index.html) and search for information about **liblo** and other OSC libraries.
### OSC Message Basics
The OSC specification supports the ability to send collections of data (e.g. floats, strings, large integers, etc.) tagged with string-based tags called *paths*.  Most of the time, these paths are used to group messages which the client wishes to be *handled* by the same server mechanism (usually called a *handler*).  The only rule for path formation is that the string must follow the form of a traditional UNIX path:  An initial forward slash (**/**) plus a string followed by zero or more additional sets of forward slashes followed by strings.  The following are legal paths:

```C
/myfirstpath
/my/first/path
/my/very/long/path
```
and these are illegal:

```C
anillegalpath (missing opening /)
/             (empty path)
//mypath      (empty first path segment)
```

For most OSC messages, the path is followed by a string indicating the format of the data elements being sent in the message.  For example, "sdd" indicates that a string and two double-precision floating point numbers are being sent.

### <a name="native-messages"></a>Native RTcmix OSC Messages
RTcmix automatically handles several messages tagged with system-specific paths.  No other OSC server would be expected to understand these, and the paths are constructed accordingly:

- **/RTcmix/ScoreCommands**  
This is used to send a Minc score.  It uses a single "s" to indicate that a string follows.  This is very powerful and is used to send portions of or entire Minc scores to RTcmix, which is functionally identical to running "CMIX -f myMincScore.txt" from the command line in non-OSC mode. 
* **/RTcmix/quit**  
This is used to shut down the server entirely.  It will cause the **CMIX** executable to exit.  It takes no additional arguments.  Use this only for emergency situations!
* **/RTcmix/stop**  
This is used to stop the CMIX audio process, which may have been previously started via a call to [rtsetparams](../reference/scorefile/rtsetparams.md).  This is sometimes useful to allow the audio format to be changed.  It takes no additional arguments.
* **/RTcmix/\<Minc command\>**  
This message allows you to call almost any built-in (i.e., not user-defined) CMIX Minc function call.  For example, to start audio via a call to [rtsetparams](../reference/scorefile/rtsetparams.md), you would send a message such as:

	```C
	/RTcmix/rtsetparams "ddd" 44100 2 1024
	```
This would result in a direct, immediate call to **rtsetparams(44100, 2, 1024)** on the CMIX server.

### Standard OSC Message Processing in RTcmix
In addition to the custom native messages described above, RTcmix is designed to listen for and process a wide variety of OSC message formats.  As described above, and in any decent OSC documentation, a well-formed OSC message consists of a text-based tag called a path, followed by a description of the format of each of the following arguments, followed by arguments which match that description.  Here is a table of OSC types and what their equivalent is when processed by RTcmix:

OSC Tag	| OSC Type	| RTcmix Type
--------	|----------	|------------
i			| int32		| float
f			| float		| float
s			| string		| string
b			| blob			| **not supported**
h			| int64		| float (as long as it does not exceed DOUBLE_MAX)
t			| time tag	| **not supported**
d			| double		| float
S			| symbol		| string
c			| char			| string (i.e., 'c' becomes "c")
m			| 4 byte midi | **not supported**
T			| true			| float set to 'true' (1)
F			| false		| float set to 'false' (0)
N			| Nil			| **not supported**
I			| Infinitum	| **not supported**

The number of supported types is likely to expand as use cases are discovered.

**Note:** It is important to remember that what Minc calls a "float" is actually a double-precision floating point, and so has the accuracy and range of a double.

## Using RTcmix as a Remote Synthesis/Processing Server

### Message processing vs. handling
We use the word "handling" as distinct from "processing": If RTcmix can *process* a message, it indicates that it accepts it as a valid message (i.e., the low-level layer received it without error and it contains no unsupported types).  When RTcmix *handles* a message, it means that there is code on the server side which *takes some form of action* in response to the message.  If the low-level OSC layer accepts the incoming message, RTcmix is guaranteed at minimum to take a default action in response to receiving the message.  This is called the **default message handler**.

### RTcmix message handling
#### Introduction
Before this discussion goes further, it is essential to remember that at its heart, RTcmix is a *score-driven* system.  This means that to get RTcmix to *do* anything, it must parse a text *score* whose contents it will execute according to the rules of the [Minc](../reference/scorefile/Minc.html) language.
#### Message Translation: OSC -> Minc
Because RTcmix only responds to the actions described by the Minc language, all incoming OSC messages must be translated into lines of Minc script before they can be parsed and acted upon.  This is why there are limitations and conversions in the accepted set of data types above.  For example, among others, Minc has no integer, single-precision float, or character data types, so all incoming data of those types must be converted into another form.
#### Argument list -> Minc array
All arguments passed in via an OSC message are converted into supported Minc types as described above, and then they are added as elements to a Minc array (or list).  Because these arrays allow elements of any supported Minc type, all the OSC arguments appear in the array in a predictable format and in the same order as they appeared in the original message.  See below for more about how this works.
#### Minc User-defined Functions as message handlers
**Note:** Before you read this section, make sure you are completely familiar with the use of user-defined Minc functions and function pointers as discussed in detail [here](../reference/scorefile/Minc.html#minc-functions) and [here](../reference/scorefile/Minc.html#mfunction).<br>
The power and flexibility of RTcmix as an OSC server comes from the manner in which it binds OSC message paths to user-defined Minc functions.  Every standard OSC message can be associated with a unique message handler, and each handler is a Minc function defined by the client (you).  These handlers can be named anything you wish as long as it follows the Minc language rules.  Their function signature *must* match the following example:

```C
float myMessageHandler(list arglist)
{
	// look at arguments
	// do something
	return 0;		// or 1 if failure
}
```
1. Each must return a float value, usually set to the status of the operation (0 == success).
2. Each must take a single list (array) as the sole argument.  This is the array mentioned above which contains all the possibly-converted arguments.  The *name* of the argument list variable can be anything you chose, of course.

And here lies the power and flexibility:  In this handler (or any other handler), *you can do whatever you want* that is legal Minc code.  You could generate an entire 20-minute composition if you chose to!  Whatever is possible within a Minc score file is possible here.
#### Defining and Registering Minc message handlers
So how do you get RTcmix to know about these handler functions and use them when messages come in?  When RTcmix is run in OSC server mode, it automatically loads a small set of Minc utility functions (see below) which you use to register your handlers.  To get this all to work together to set up your server, you create what we call a *training score*.  This is essentially a Minc score which *defines your handler functions* and *registers them to be called when specific OSC message paths are sent*.  This training score is itself sent to the RTcmix server using the custom /RTcmix/ScoreCommands path described [above](#native-messages).  Any number of handlers may be defined in these scores, and any number of training scores may be sent to the server.  The server may be "re-trained" at any time.
#### Example of a training score
```C++
// First we define the Minc function we wish to register, following the rules above

float myFirstMessageHandler(list args)
{
	printf("myFirstMessageHandler was called with args %l\n", args);
	return 0;
}

// oscRegisterMessageHandler() is the utility for binding a path to a handler

oscRegisterMessageHandler("/sometag", myFirstMessageHandler);

```
Once this score is sent to RTcmix, **any** OSC message with its path set to "/sometag" will call this handler.  'args' will contain the contents of the arguments sent via OSC.  If there were no arguments (which often happens), the list will be empty.<br>

If you wish to replace this handler with a new one, just repeat the above with a new function.  If you wish to un-register a handler, you would send a score with something like the following:

```C++
oscUnregisterMessageHandler(myFirstMessageHandler);

```
#### Interpreting the contents of the argument list
It will be useful to familiarize yourself with Minc lists/arrays [here](../reference/scorefile/Minc.html#list) and [here](../reference/scorefile/Minc.html#more-about-arrays).  Though there is an assumption that because you have registered a specific handler you know what order and type each passed argument is, it is possible to build error-checking into your handler system:

```C++
float myErrorCheckingHandler(list args)
{
	// This handler expects precisely 3 arguments
	if (args.len() != 3) {
		printf("myErrorCheckingHandler: wrong number of args!\n");
		return 1;		// signals an error
	}
	// Likewise, the handler expects the first argument to be a string
	if (args[0].type() != "string") {
		printf("myErrorCheckingHandler: expected string in first argument!\n");
		return 1;
	}
	// etc.
	// If everything is good...
	// Do the handling, then...
	return 0;
}
```

Here we have checked for argument count and an example of an argument type check.  You can of course do more specific checking for ranges, etc., just like you would for a Minc function in a score-driven system.

#### The Default message handler
Sometimes it is useful to have a catch-all handler which responds to any path that you have not specifically registered a handler for.  To do this in RTcmix, create and send a training score like the following:

```C++
float myDefaultMessageHandler(list args)
{
	// Do something with these args
	return 0;
}

oscRegisterDefaultMessageHandler(myDefaultMessageHandler);
```

In similar fashion to other message handlers, you can unregister this function by sending a score like the following:

```C++
oscUnregisterDefaultMessageHandler();
```
