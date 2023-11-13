---
title: Minc
layout: ref
---

## Minc

*The default scorefile parsing/interface language for RTcmix*  

### Introduction to the **Minc** Parser

**Minc** is the default parsing interface for RTcmix. Invoking the command

```cpp 
CMIX < some_score.file
```
or

```cpp 
CMIX -f some_score.file
```

will use **Minc** in "standalone" mode to parse the score and make the appropriate function
calls to run RTcmix.  The following are other ways of invoking the **Minc** parser:

  - [embedded](../../tutorials/embed.html) within another application
  - through a [TCP/IP socket](../../tutorials/socket.html), 
  - via [Open Sound Control](https://ccrma.stanford.edu/groups/osc/index.html) (OSC)

There are other parser options, including the ability to
run RTcmix from [Perl](../../tutorials/perl.html) and [Python](../../tutorials/python.html).

**Minc** is also
used to parse buffer-scripts in the Max/MSP
[rtcmix\~](../../rtcmix_/index.html) object and in the
[iRTcmix](../../irtcmix/index.html) package for iOS devices.

**Minc** is an interpretive (non-compiled), interactive language.  It takes much
of its functionality from the **C** programming
language, including flow-of-control features such as *for* and *while*
loops, *if* decision-constructs, etc. In addition to the many examples below,
see the [RTcmix tutorial](../../tutorials/standalone.html) (especially the later
sections about algorithmic composition) for examples of **Minc** use.
One of the main differences between **Minc** and **C** is the ability to
leave out semicolons at the end of lines (note: semicolons are still required
in "for(...)" statements and a couple of other places). Semicolons may be used
at the end of **Minc** lines, but they are not required. The advantage of using them
is that the parser can more easily pinpoint the offending line number in your score - very
useful for large, complex scores!

### Some useful features of **Minc**

- **Minc** Operators modify or combine the value of **Minc** variables:
	-  '+' add
	- '-' subtract
	- '\*' multiply
	- '/' divide
	- '^' and '**' power
	- '++' increment (add 1)
	- '--' decrement (subtract1)
	- '%' modulo
		
	Many of these are also available in the form '+=', '-=', etc.
		
- **Minc** Functions may be calls to Instruments, built-in, or score-defined
    functions used to perform score file calculations. Return values are assigned to **Minc** 
    variables with the '=' assignment operator:
    
	```cpp
	aval = somecalculation(anotherval, something_else)
	    
	thisone = thatone + those
	    
	ANINSTRUMENT(with, many, different, parameters)
	```
    
	In addition, operations and functions may be embedded (or 'nested'):
	    
	```cpp 
	ANINSTRUMENT(somefunction(var1, var2), val^3.2, aval, 3.1654, func1(func2(array[3])))
	```

- Several **C** conditional-branching statements are included in
    **Minc** ("if/then/else", "while"). The standard **C** comparison
    operators ('==', '&&', '||', '\!=', '\>', '\<', etc.) are used for
    the conditional comparison. Using parentheses to group and determine
    precedence of evaluation is also supported:
    
	```cpp 
	if ((val > 2.0) && < (val2 != 0)) { ... }
	```
    
    Note that the "do...while" and "switch" statements are not supported
    in **Minc**.  Postfix (x--) operators are also not supported.
      
- Comments may be included in **Minc** scripts using **C** comment
syntax:
    
    ```cpp 
    /* a comment may be between two slash-asterisk markers */
    
    /* and it
    	may also span
    	several lines
    */
    
    // anything after two backslashes on a line will be construed as a comment
    ```

### **Minc** Data Types

Data types are different “flavors” of variables used to store different kinds of information. One of the other major differences between **C** and **Minc** is the set of built-in data types available. Variables of the following types can be created in your scores, either by declaring them directly or having them automatically declared.

#### <a name="float"></a>float

**floats** are all numbers such as -15.4 and 0.0125, and includes whole number (integer) values like 3 or 99.  All are stored and passed around as floating point. They can be declared and then set:

```cpp
float instVolume;
instVolume = get_inst_volume()
```
	
or auto-declared:

```cpp
instVolume = 7
instPitch = some_function_which_returns_a_float();
```

Note that all **Minc** numerical values are floating-point.  Care is taken to prevent round-off errors
from causing problems for counting variables that would normally use integers. The
[trunc](trunc.html) and [round](round.html) built-in functions can be used to
guarantee a correct 'integer' value.

#### <a name="string"></a>string

**strings** are any form of text, like comments, file names, names of instruments, etc.  These are stored in string variables. Like floats, they can be declared:

```cpp
string soundPath;
```
	
or auto-declared:

```cpp
soundPath = “/tmp/recording.wav"
soundPath = some_function_which_returns_a_string();
```

Strings can be concatenated using the ‘+’ operator:
	
```cpp
sentence = “hello" + “ “ + “world!"
print(sentence)
    “hello world!"
```
#### <a name="list"></a>list

**lists** or, as they are often called, **Minc arrays**, store a set of variables of any type which can be accessed by indexing into the list using the [] operator. These are extremely useful in a score. The following are acceptable Minc constructions:

```cpp
// auto-declare an array filled with floats
a = { 1, 2, 3, 4, 5 }
	
// use various indices into that array as inst args
INSTRUMENT(a[3], a[0], ...)
	
b = {} // this auto-declares an empty array
for (i = 0; i < 10; i += 1) {
    b[i] = i * 3		// the array will grow as elements are added
}
```
For more information, see [More About Arrays](#more-about-arrays) below.

#### <a name="struct"></a>handle

**handles** are a type used to store references to special objects returned from other **Minc** functions such as maketable() and makeinstrument(). Quite often variables of this type will be handed to Instrument calls to set up dynamic p-fields.
Handles to RTcmix tables have the extremely powerful ability to be combined with each other and with other arithmetic operators to create new tables which can then be accessed using functions like samptable():

```cpp
// create a line table between 0.0 and 1.0
line = maketable(“line", “nonorm", 10000, 0,0, 1,1)
	
// ‘line’ is now a Minc handle referring to that table
	
print(samptable(line, 10000))
	1   // last value in table
	
lineTimes7 = 7 * line;   // it’s this simple
print(samptable(lineTimes7,10000))
	7    // last value in table, now 7 x what it was
```

#### <a name="struct"></a>struct

**structs** allow you to define and use custom data types which are very much like the "struct" concept in the **C** and **C++** languages:  It allows you to group a bunch of variables together into a named container which you can pass around. In **Minc**, like in **C**, a struct type must be named as part of its definition: 
	
```cpp
struct MyData { ... }
```
What goes between the curly braces is called a member list, and it consists of a comma-separated list of variable declarations:

```cpp
struct MyData { string name,
                float number,
                list array
              }
```
These members can be any **Minc** data type, and there is no limit on the number of member variables. Their names must all be unique within the struct definition. For now, structs cannot be nested within each other. To declare a variable in your score to be a struct, just use the struct type as follows:

```cpp
struct MyData instrumentData
```
To assign and read from the members, use the "dot" syntax:

```cpp
instrumentData.name = "Loud Instrument"
instrumentData.number = 1
instrumentData.array = {}
	
printf("Instrument %f: %s\n", instrumentData.number, instrumentData.name)
```
For more details, see [More About Structs](#more-about-structs) below.

#### <a name="map"></a>map

**maps** in **Minc** allows you to use one piece of score information as a key to access another. A simple example should help explain. Imagine you have a set of sound files that you wish to use in your score, but you have specific durations you want to read from them (i.e., not their full duration). You can create a map to associate the paths to the files to the durations you want:

```cpp
map durationMap;  // creates empty map
	
// Assign three duration floats into the map using three string keys
	
durationMap[“lion_roar.wav"] = 3.0
durationMap[“bird_tweet.wav"] = 8.2
durationMap[“gunshot.wav"] = 0.5
	
// later, open and play one of these using one of these mapped durations
	
currentSound = “lion_roar.wav"
rtinput(currentSound)
	
MIX(inskip=0,
    outskip=0,
    dur=durationMap[currentSound],  // get duration by looking up via the key
    gain=1000,
    0, 1)
```
**Minc** maps are extremely flexible; both the stored item and the key *can be any type* supported by **Minc**.

#### <a name="mfunction"></a>mfunction

**mfunction** supports an advanced feature which allows a score to pass score-defined **Minc** functions around as variables - which could then be passed as an argument to another function, or stored in a **Minc** array, etc. See the section [Score-defined Minc Functions](#minc-functions), below.

### <a name="declaring-vars"></a>A note about declaring variables

**Minc** differs from **C** in how variable declarations work.  In the latter, the declaration of a variable and the assignment of a value to it can happen in a single statement:

```cpp
// C ALLOWS (AND PREFERS) THIS
float volume = 10000;
```
in **Minc**, when you *auto-declare* a variable, the parser "knows" what type it is by what you assign to it:

```cpp
x = "A string"		// x is now a string variable
y = { 1, 2, 3 }		// y is now a list variable
```
Explicit (non-auto) declarations in **Minc** must be just a bare declaration:

```cpp
float x, y, z, q		// x, y, z, q all declared to be float variables
string s = "Hello"		// THIS WILL GENERATE A COMPILER ERROR
```
The purpose of the explicit declarations will be discussed later.

### <a name="more-about-arrays"></a>More About Arrays


- **Minc** arrays can contain 'mixed' data types:
	
	```cpp
	afloat = 1.2345
	astring = "hey hey!"
	b = { 123, afloat, "ho ho", astring }
	```
- Arrays can contain pfield-handle handles for tables, etc.:

	```cpp
	e = {}
	x = 1
	for (i = 0; i < 10; i += 1) {
	    e[i] = maketable("random", 1000, "linear", min=x, max=x*2, seed=x*3)
	    x *= 2
	}
	```

- The [len](len.html) built-in function is used to determine the length of an array (as well as lengths of other **Minc** data-types).

	```cpp
	f = 90
	str = "hello"
	mylist = { 1, 2, 3 }
	
	print(len(f))
	print(len(str))
	print(len(mylist))
	
		1
		5
		3
	```

- Arrays can be passed as an argument to any built-in function or instrument call. When this is done, the elements in the list become the next N arguments to the function:

	```cpp
	// Note: To be passed as function arguments, all the array's items must be floats
	remaining_args = { 0,0, 1,1, 3,1, 5.5,0 }
	
	// This next line is the same as calling maketable(“line", 1000, 0,0, 1,1, etc.
	env = maketable("line", 1000, remaining_args)
	```

- Array elements may also be other arrays, and can be of different lengths. To access the sub-arrays, you “double-index” the "parent" array:

	```cpp	
	arr1 = { 1, 2, 3, 4, 5, 6, 7 }
	arr2 = { 77, 87, 97, 107 }
	superarr = { arr1, arr2 }
	for (i = 0; i < len(arr1); i += 1) {
	    print(superarr[0][i])
	}
	```
 
- Arrays can be concatenated using the '+' operator: 

	```cpp
	first = { 1, 2, 3 }
	second = { 4, 5, 6 }
	third = first + second
	print(third)
		[1, 2, 3, 4, 5, 6]
	```
- All the elements of an array can be modified via operators +, -, *, and / followed a float variable:

	```cpp
	arr = { 1, 2, 3 }
	arr = arr + 10		// add 10 to all elements
	print(arr)
		[11, 12, 13]
	arr = arr * 100		// multiply all elements by 100
	print(arr)
		[1100, 1200, 1300]
	```
	Currently the ++ and -- operators do not work on an array or an element indexed on an array:
	
	```cpp
	arr = { 1, 2, 3 }
	++arr		// NOT OK
	++arr[1]	// NOT OK
	```

### <a name="more-about-structs"></a>More About Structs


- All struct member variables are set to 0 or NULL by default. You cannot auto-declare struct variables unless you are assigning from one to another:

	OK:

	```cpp
	struct MyData someData;
		
	// initialize members of 'someData'...
	someData.someMember = 9;
		
	copyOfMyData = someData		// 'copyOfMyData' and 'someData' now point at the *same* struct object
	```
	
	NOT OK:
	
	```cpp
	autoStructData.name = "something" // cannot auto-declare 'autoStructData'!!
	```

- Struct variables may be used as arguments and return types for custom functions. This is a very convenient way to get lots of different variables into [custom function calls](#minc-functions) without having to create long lists of arguments. 

	```cpp
	struct EasyArgs { float outskip, float inskip, float dur, float amp }
		
	float myMixWrapper(struct EasyArgs theArgs)
	{
		return MIX(theArgs.outskip, theArgs.inskip, theArgs.dur, theArgs.amp, 0, 1)
	}

	struct EasyArgs args = { 15.4, 0, DUR(), 10000 }  // Note this quick-hand for initializing members!!
	
	myMixWrapper(args)
	
	args.outskip = 22.8;	// tweak value of one arg
	
	myMixWrapper(args)		// call it again with new outskip

	```

### <a name="minc-functions"></a>Score-defined (custom) **Minc** Functions


  - **Minc** supports the ability to create custom functions right in
    your score. The format for these functions is nearly identical to
    that used to define functions in the **C** programming language:
    
	```cpp
	return_type function_name(arg0_type arg0_name, arg1_type arg1_name, ..., argN_type, argN_name)
	{
	    <function body>
	}
    ```
    Rules for Creating and Using Custom Functions:
    
      - The argument types can be any type supported by **Minc**
      - A function can have any number of arguments, including none.
      - *A function must return a value of one type or another.* Use a dummy '0'
        if nothing else (and have the function returns a float).
      - The type of the variable returned must match the return
		type of the function.
      - Use the 'return' keyword to mark the place where the value
        should be returned. There can be more than one such place (e.g.
        inside if() blocks).
      - The arguments passed to the function must match the declared
        types (i.e., no type conversion supported).
      - If the score provides fewer arguments that the function expects,
        the remainder will be set to 0 (or NULL). This is legal but you
        will get a friendly warning.
      - There can only be one function defined with any given function
        name.
      - The body of a function may contain any legal Minc score
        commands.
      - Functions can call other functions, including themselves
        (recursively).
      - A function must be defined before (i.e., above) the place where
        it is first called (except when it is calling itself).
      - Functions can be defined in a separate file which is pulled into another score using [include](#the-include-statement). This makes it easy to create and re-use function "libraries"\!
      - Variable visibility rules. These are a bit complicated.
          - Any variable declared globally in the main score (outside of
            any function) is visible inside the body of all functions.
          - This is true even if that global variable is declared after
            the function is declared in the score, as long as it is
            declared before the function is called.
          - Any variable declared inside a function is not visible
            outside of the function.
    
    Here is an example of some of these rules:
    
    ```cpp
    varDeclaredAboveFun = 89;		// this is visible to the function
    
    // A custom function declared to take a list and return a float count of its items
    
    float countList(list inListArgument)
    {
    	// Here we reference the var declared below the function to show it works
    	printf("%s - counting the list\n", varDeclaredBelowFun);
    	
    	// declaring these here guarantees that these variables are local to this function
    	float listLen, itemCount;	

    	listLen = len(inListArgument);
    	itemCount = 0		// **Minc** always zeros variables, but this is good practice

    	// If the list contains lists, the function is called recursively on those.

    	for (i = 0; i < listLen; ++i) {
    		item = inListArgument[i];
    		// Use the type() function to look for list items
    		if (type(item) == "list") {
    			itemCount += countList(item);
    		}
    		else {
    			++itemCount;		// "++" operator is a shorthand for "+= 1"
    		}
    	}
    	return itemCount;
    }
    
    varDeclaredBelowFun = "hi there";	// this is *also* visible to the function
    
    listToCount = { 3, 19, 4, { 1, 2, 3 } };  // list containing a list
    
    // Here I include the complete printout from **Minc** so you can see the recursive call
    // happening
    
    listCount = countList(listToCount);
	    ============================
	    countList: [3, 19, 4, [1, 2, 3]]
	    hi there - counting the list
		============================
		countList: [1, 2, 3]
		hi there - counting the list
	
	print(listCount);
		6
    
    varDeclaredBelowFunctionCall = {}	// but this variable is not visible to the function
    ```
  - Custom functions can be stored into variables, lists, maps and structs using the [mfunction](#mfunction) data type discussed above:

	```cpp
	// a custom function
	float pluralFunction(string arg) {
	    combinedString = "Many " + arg + "s";		// Making use of the "+" operator to concatenate strings
		printf("%s\n", combinedString);
		return 0;	// dummy return
	}
	
	// call the function to see it work
	pluralFunction(“cat")
		Many cats
	
	// assign the function to a variable
	funVar = pluralFunction	// ‘funVar’ is now an mfunction
	
	// call the function on the newly-created mfunction variable
	funVar(“meatball")
		Many meatballs
	```
      
### More Useful Minc Commands and Features

#### <a name="the-include-statement"></a>The 'include' statement

The include statement can be used to embed one (or more) **Minc** score files within another.  This can be very helpful in creating a single file where parameters (like sampling rate and audio device) are set and then included by multiple other scores.  Changing the values in the one file will then affect all files which include it.

Here we create a score file to be included in other files.  This file calls [rtsetparams](rtsetparams.html) for us, and makes two global variables available to any score which includes this one.  We'll name this "setup\_48K\_stereo.sco".

```cpp
sample_rate = 48000
channel_count = 2
rtsetparams(sample_rate, channel_count)

```
When we include that score in another score, the **Minc** parser will automatically run the commands in the included score and set those variables.

```cpp
include "setup_48K_stereo.sco"

rtinput("some_file.wav");	// open a file
pan = 0;	// default value

// Adjust the pan value only if we are running in stereo.  If you changed your include to, perhaps,
// "setup_48K_monaural.sco", you would set channel_count to 1 in there, and this score would automatically
// do the right thing.

if (channel_count == 2) {
	pan = 0.5;
}
STEREO(inskip=0, outskip=0, DUR(), 1, pan)
```

#### <a name="command-line-named-args"></a>Command-line "named arguments" ("standalone" mode only)

Number and string values can be specified on the ‘CMIX’ command line and used as named variables within the score.  This lets you create one score with named argument variables for all its important parameters, and then you can experiment with changing those parameters on the command line.  So, one score instead of 25 separate scores!

With a command line execution like this:

```cpp
CMIX —-sample_rate=48000 —-filename=“input.wav" < scorefile
```
and a score set up like this:

```cpp
// ‘$’ marks a variable as something set via the command line
rtsetparams($sample_rate, 2);
rtinput($filename);
```

**Minc** will set $sample_rate to 48000 and $filename to “input.wav”.  It’s an error to use a $variable you have not specified on the command line, but you can check if it has been set by using the boolean expression ?variable:

```cpp
rate = 44100;  // this will be the default
if (?sample_rate) { rate = $sample_rate } // use '$sample_rate' if set
```
You can even pass in a list via named argument!  You just have to remember to double-quote the entire list. With a this score:

```cpp
printf("The passed-in list was: %l\n", $listargument);
```
This command runs as follows.  Note that the string element is also not internally quoted:

```cpp
CMIX -f listarg.sco --listargument="{1, 2, Buckle My Shoe}"
The passed-in list was: [1, 2, "Buckle My Shoe"]
```

#### type()

The [type](type.html) command returns a **Minc** string which specifies the **Minc** data type of a particular variable:

```cpp
myList = { 1, 2, 3 }
print(type(myList))
	“list"
```

#### index()

The [index](index.html) command is also very useful with **Minc** arrays., and reports the particular index in the array which holds an item.

```cpp`
foo = "foo";
bar = "bar"
afloat = 1.2345;
astring = "blah"

b = { 123, afloat, "blabber", astring, foo + bar } // mixed array

// also note the string concat with '+' operator for the last item

idx = index(b, astring) // Return a zero-based index into array (arg 1) of item (arg 2)
print(idx)
	3
```

[index](index.html) returns -1 if the specified item is not found.

#### printf()

Minc supports a simplified version of the formatted [printf](printf.html)() as found in **C** and **C++**

```cpp
foo = "foo";
bar = "bar"
afloat = 1.2345;
astring = "blah"

b = { 123, afloat, "blabber", astring, foo + bar } // mixed array
numitems = len(b) // get length of array

// format for floats and strings
printf("afloat=%f, astring=%s\n", afloat, astring)

// format to tream a float as an integer value
printf("array b: %d items: %l\n", numitems)

// format lists (i.e., arrays), structs, maps, and function pointers with special character "%z"
printf("array b: %z\n", b)
```

### **Minc** error handling

When **Minc** encounters a parsing error, it generally exits
(for command-line CMIX) or returns a FATAL\_ERROR value (embedded apps like
[rtcmix\~](../../rtcmix_/index.html) or
[iRTcmix](../../irtcmix/index.html)) with an attempt to identify
the number of the line with the syntax error. As mentioned above, supplying
semicolons at the end of score lines will assist in accurately determining the
line location for the error.

If you wish to do your own additional error checking in your score, you can use the error() function:

```cpp
if ($entered_sample_rate < 8000)
{
	error("entered sample rate (%f) is an illegal value!")
}
```

The parsing of your score will stop at that point and FATAL\_ERROR will be returned.

## Advanced Topics in **Minc**

**Minc** has a set of extremely powerful and flexible features which may take a bit of extra work to understand if you do not have a programming background.  Some of these are adaptations of features found in the **C++** programming language.  There are an infinity of things you can do with the system as already described, so if you are interested in these, study the provided examples.

### Structs as "objects"
For programmers, the term "object" usually refers to a chunk of information (the "state") which moves around together as a unit (like the structs discussed above), but also which provide a set of object-specific functions (often called "methods") which allow a user to get and modify that "state".  **Minc** supports the ability to expand the definition of a struct to include "method functions".  These functions are different than other "user-defined" function because:

  - they are declared as part of the struct's declaration
  - they are called using the "dot" operator on a variable that has the struct type
  - they can access, modify, and return the value of any of the member variables that are part of the struct

```cpp
// Simple struct object which contains a count

struct Counter
{
	float _count,			// all member variables and method function decls are comma-separated
	method float set(float initialCount)
	{
		this._count = initialCount;	// member variables are always accessed as "this.membername"
		return this._count;
	},
	method float increment()
	{
		this._count = this._count + 1;
		return this._count;
	}
}

struct Counter myCounter		// declare one of these objects
myCounter.set(100)				// counter will start at 100

print(myCounter.increment())
print(myCounter.increment())
print(myCounter.increment())

	101
	102
	103

```

## History of the Minc Parser

The original Minc parser was coded by Lars Graf. Additional modifications were made by John Gibson, Doug Scott and others.

The following is from the original cmix **Minc** documentation. Some is a bit dated, but the basic concepts are still valid. For a more complete and current grammar, see the RTcmix/src/parser/minc/minc.y file

---

Minc (Minc is not c) is the data specification language for cmix. It was written by Lars Graf. Minc almost follows a C-like syntax. Arithmetic, conditional and logical expressions are permitted, as well as looping. Any statement of the form foo(val, num, etc) results in the execution of foo() if it is a cmix procedure and declared as such, either in the user's profile.c or in cmix's ug_intro.c. If it is neither the evaluated terms within parentheses will simply be printed and returned. The arguments within the parentheses are passed to the cmix procedure in its float \*p array. All variables must be declared as float. The sign ; has the meaning 'clear out any syntax errors'. cmix routines such as open, input, output which have a file name as a first argument, must pass that argument between " signs. Minc, has two modes of operation: batch and interactive (the default). The only difference between them is that loops cannot be executed in interactive mode. To run in batch mode a -b flag must be specified on the Cmix command line, and all data must be entered either by redirecting input from a file, or by typing all data followed by a <cntrl-D>. Comments are inclosed by /\* and \*/ as in C. Any user function which returns a value can return that value to Minc if it is declared as a type double and introduced in the profile.c file.

---

The following is Lars Graf's formal statement of the language:

```cpp
     Order of precedents:

     ||
     &&
     =  !=
     < >  <= >=
     + -
     * /
     **  ^


     CASTS

     Syntax:

     prg: stml

     stml :        stmt
          |   stml  stmt
          |   stmt ;
          |   stml  stmt ;

     stmt:         float idl           /* declare ids */
          |   id  = exp
          |   IF bexp stmt
          |   IF bexp stmt ELSE stmt
          |   WHILE bexp stmt
          |   FOR ( stmt , bexp , stmt ) stmt
          |   id ( expl )     /* a call to a cmix function  */
          |   { stml }


     idl:     id              /* like: fnct_name(arg1,arg2,arg3) */
          |   idl , id

     id:   any letter/digit/"_" combination, but start with letter

     expl:       exp
          |   exp ',' expl
          |   or no function argument

     bexp:         exp
          |   ! bexp
          |   bexp &&  bexp
          |   bexp ||  bexp
          |   bexp = bexp   /* note the comparison of booleans */
          |   exp < exp
          |   exp > exp
          |   exp != exp
          |   exp <= exp
          |   exp >= exp
          |   TRUE
          |   FALSE

     exp:     exp ** exp
          |   - exp
          |   exp * exp
          |   exp / exp
          |   exp + exp
          |   exp - exp
          |   ( bexp )
          |   any number: e.g. 34 -645.34 234.E23  ...


     Note:

     1)  Semicolon and  are optional.

     2)  '=' is used for assignments and boolean expressions.

     3)  False = 0; True = 1(or nonzero).




     Samples:

     /*  this is a legal comment  */

     float foo,x;

     if i=3 && foo>4  say_mumble( foo**i , i-foo*3)
          else mumble_foo()


     while x=3 {

          this_is_a_cmix_function_call()
          x= 5 * (foo=i)     /* this is slime */

     while true nested_loops_are_possible()
     }


     for (x=1, x <= 10,x=x+1) dispatch(x)


     Minc also has the following facilities:

     1)  function calls can return values

          e.g.      i = rand(j)

          NOTE:  those are calls to your own C-functions.
                 You may adjust your dispatcher and your functions accordingly
                 The function must be introduced in the profile, and must
                 be of type double.

     2)  assignment statements have the value of the assignment.

          e.g.      i = j = 5
                 while (i=j) == 5  { mumblings .. }

     3)  strings can be passed as arguments.

          e.g.      file_id = open("name of file",2)
                    printf("some string %f %f0,i,j)
               /* of course, the dispatcher has to know about printf */

          NOTE:   the string pointer is passed as a double; to convert
               back, you have to first cast it into an integer, and then
               you have to cast the integer into a char*.

     4)  use '=' for assigment; and '==' as the booleam equal operator.


     Here is a simple example file:

     open("sf/bigsound",0,0)
     open("sf/bigmix",1,2)
     sfclean(1)
     float a,b,c,d
     a=2 b=3 c=28000 d=-4
     sound(a,b,c,1,22*19,d=d-a)
     sound(a=a+2, b+.1, 99, d=d-a+2)
```
