---
title: Minc
layout: ref
---

### Minc

*default scorefile parsing/interface language for RTcmix*  
  
**Minc** is the default interface for RTcmix. Invoking the command

``` 
       CMIX < some_score.file
```

will use **Minc** to parse the score and make the appropriate function
calls to run RTcmix. There are other options, including the ability to
run RTcmix from [perl](../../tutorials/perl.html),
[python](../../tutorials/python.html),
[embedded](../../tutorials/embed.html) within another application, or
through a [TCP/IP socket](../../tutorials/socket.html). **Minc** is also
used to parse buffer-scripts in the Max/MSP
[rtcmix\~](../../rtcmix_/index.html) object and in the
[iRTcmix](../../iRTcmix/index-2.html) package for iOS devices.

**Minc** takes most of its functionality from the C programming
language, including flow-of-control features such as *for* and *while*
loops, *if* decision-constructs, etc. See the [RTcmix
tutorial](../../tutorials/standalone.html) (especially the later
sections about algorithmic composition) for examples of **Minc** use.
The biggest differences between **Minc** and C is the lack of pre- and
post-fix operators in **Minc** (i.e., "i++" or "--counter") and the
absence of semicolons as line/statement terminators. (Note: semicolons
are still required in "for (...)" statements.) Semicolons *may* be used
at the end of **Minc** lines, but they are not required.

Some other useful features of **Minc**:

  - Combination assignment operators are supported. This means that
    expressions such as:
    
    ``` 
       for (i = 0; i < 7; i += 2) { ...
    ```
    
    and
    
    ``` 
       while (asympt > 0.001) { asympt *= 0.5; ...}
    ```
    
    will be parsed correctly.  
      

  - The **Minc** array facility is very useful. The following are
    acceptable **Minc** constructions:
    
    ``` 
       a = { 1, 2, 3, 4, 5 }
       INSTRUMENT(a[3], a[0], ...)
    
       b = {} // note that "b" is dimensioned by use
       for (i = 0; i < 10; i += 1)
          b[i] = i * 3
    ```
    
    **Minc** arrays can also contain mixed data types:
    
    ``` 
       afloat = 1.2345
       astring = "hey hey!"
       b = { 123, afloat, "ho ho", astring }
    ```
    
    Arrays can also contain
    [pfield-handle](../instruments/pfield-enabled-2.html) handles for
    tables, etc.:
    
    ``` 
       e = {}
       x = 1
       for (i = 0; i < 10; i += 1) {
          e[i] = maketable("random", 1000, "linear", min=x, max=x*2, seed=x*3)
          x *= 2
       }
    ```
    
    Arrays can be passed into [maketable](maketable.html) constructions:
    
    ``` 
       d = { 0,0, 1,1, 3,1, 5.5,0 }
       env = maketable("line", 1000, d)
    ```
    
    Array elements may also include other arrays. However, to access the
    sub-arrays, a temporary array variable is necessary:
    
    ``` 
       arr1 = { 1, 2, 3, 4, 5, 6, 7 }
       arr2 = { 77, 87, 97, 107 }
       superarr = { arr1, arr2 }
    
       temparr = superarr[0]
       for (i = 0; i < len(arr1); i += 1) {
          print(temparr[i])
       }
    ```
    
    "temparr" in the above code temporarily represents the "arr1" array.
    Note that arrays of different lengths may be stored in a "super"
    array.
    
    The [len](len.html) built-in function is used to determine the
    length of an array (as well as lengths of other **Minc**
    data-types).
    
    Arrays can be concatenated using the '+' operator:
    
    ``` 
        first = { 1, 2, 3 }
        second = { 4, 5, 6 }
        third = first + second
        print(third)
        [1, 2, 3, 4, 5, 6]
    ```
    
    All the elements of an array can be modified via operators +, -, \*,
    and / followed a float variable:
    
    ``` 
        arr = { 1, 2, 3 }
        arr = arr + 10
        print(arr)
        [11, 12, 13]
        arr = arr * 100
        print(arr)
        [1100, 1200, 1300]
    ```

  - **Minc** allows you to define and use custom data types which are
    very much like the "struct" concept in the C and C++ languages. In
    **Minc**, a struct type must be named as part of its definition:
    
        struct MyData { ... }
    
    What goes between the curly braces is called a member list, and it
    consists of a comma-separated list of variable declarations:
    
        struct MyData { string name, float number, list array }
    
    These variables can be any of the normal built-in **Minc** types,
    and there is no limit on the number of member variables. Their names
    must all be unique within the struct definition. For now, structs
    cannot be nested within each other.
    
    To declare a variable in your score to be a struct, just use the
    struct type as follows:
    
        struct MyData instrumentData
    
    To assign and read from the members, use the "dot" syntax:
    
        instrumentData.name = "Loud Instrument"
        instrumentData.number = 1
        instrumentData.array = {}
        
        printf("Instrument %s: %f\n", instrumentData.name, instrumentData.number)
    
    All struct member variables are set to 0 or NULL by default.
    
    You cannot auto-declare struct variables unless you are assigning
    from one to another:
    
    OK:
    
        struct MyData someData;
        // initialize members of someData...
        
        copyOfMyData = someData // These two now point at the same struct object
    
    NOT OK:
    
        autoStructData.name = "something" // cannot auto-declare 'autoStructData'
    
    Struct variables may be used as arguments and return types for
    custom functions. This is a very convenient way to get lots of
    different variables into your function calls without having to
    create long lists of arguments.

  - Comments may be included in **Minc** scripts using "C" comment
    syntax:
    
    ``` 
       /* a comment may be between two slash-asterisk markers */
    
       /* and it
          may also span several lines
       */
    
       // anything after two backslashes on a line will be construed as a comment
    ```
    
      

  - Actions are performed by **Minc** functions or operators ("+", "-",
    "\*", "/", "^", etc.). Functions may be Instruments or built-in
    functions used to perform scorefile calculations. Return values are
    assigned to **Minc** variables with the "=" assignment operator:
    
    ``` 
       aval = somecalculation(anotherval, something_else)
    
       thisone = thatone + those
    
       ANINSTRUMENT(with, many, different, parameters)
    ```
    
    In addition, operations and functions may be embedded (or 'nested'):
    
    ``` 
       INSTRUMENT(somefunction(var1, var2), val^3.2, aval, 3.1654, func1(func2(array[3])))
    ```
    
      

  - Several "C" conditional-branching statements are included in
    **Minc** ("if-then-else", "while"). The standard "C" comparison
    operators ("==", "&&", "||", "\!=", "\>", "\<", etc.) are used for
    the conditional comparison. Using parantheses to group and determine
    precedence of evaluation is also supported:
    
    ``` 
       if ((val > 2.0) && < (val2 != 0)) { ... }
    ```
    
    Note that the "do-until" and "switch" statements are not supported
    in **Minc**.  
      
      

  - **Minc** supports the ability to create custom functions right in
    your score. The format for these functions is nearly identical to
    that used to define functions in the C programming language:
    
        return_type function_name(arg0_type arg0_name, arg1_type arg1_name, ..., argN_type, argN_name)
        {
            <function body>
        }
    
      
    Rules:  
      
    
      - The return\_type and the argument types must be either float,
        string, handle (the type for tables, etc.), or list (the type
        for MinC arrays).
      - The function can have any number of arguments, including zero.
      - It must return a value of one type or another. Use a dummy zero
        if nothing else. The type of the variable returned must match .
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
      - Functions can call other functions, including themselves
        (recursively).
      - A function must be defined before (i.e., above) the place where
        it is first used (except when it is calling itself).
      - Functions can be defined in a separate file which is "include"d
        by a score. This makes it easy to create and re-use function
        "libraries"\!
      - The body of a function may contain any legal Minc score
        commands.
      - Variable visibility rules. These are a bit complicated.
          - Any variable declared globally in the main score (outside of
            any function) is visible inside the body of all functions.
          - This is true even if that global variable is declared after
            the function is declared in the score, as long as it is
            declared before the function is called.
          - Any variable declared inside a function is not visible
            outside of the function.
    
      
      

  - All **Minc** numerical values are floating-point, although care is
    taken to prevent round-off errors from causing problems for counting
    variables that would normally use integers. The [trunc](trunc.html)
    and [round](round.html) built-in functions can be used to guarantee
    a correct 'integer' value.  
      

  - **Minc** can also handle character-strings and the
    [pfield-handle](../instruments/pfield-enabled-2.html) variables
    (obviously\!), including operations performed on those types. The
    [stringify](stringify.html) function can be used to coerce a
    **Minc** string into a floating-point value that RTcmix instruments
    may use internally.  
      

  - The [index](index_command.html) and the [type](type.html) commands
    are also very useful with **Minc** variables and arrays. The
    [type](type.html) command can determine the **Minc** data-type of a
    particular variable, and the [index](index_command.html) can
    retrieve items at a particular index within an array. Also be awafe
    that **Minc** lists will also be 'unpacked' properly in many
    scorefile commands, such as the [print](print.html) and
    [maketable("literal"...)](maketable.html#literal) commands.
    
    ``` 
       a = { 1, 2, 3, 4, 5, 6 }
       print(a)  // print can handle arrays
    
       foo = "foo"; bar = "bar"
       afloat = 1.2345; astring = "blah"
       b = { 123, afloat, "blabber", astring, a, foo + bar } // an array can embed other arrays
          // also note the string concat with '+' operator
    
       numitems = len(b) // get length of array
       printf("array b: %d items: %l\n", numitems, b)  // printf for ints and lists (i.e., arrays)
       printf("afloat=%f, astring=%s\n", afloat, astring) // printf for floats and strings
    
       idx = index(b, 345) // Return a zero-based index into array (arg 1) of item (arg 2)
       print(idx)
    ```
    
      

  - The [include](include.html) command can be used to embed one (or
    more) **Minc** scorefiles within another.  
      

  - When **Minc** encounters a parsing error, it generally exits
    (RTcmix) or returns a FATAL\_ERROR value (embedded apps like
    [rtcmix\~](../../rtcmix_/index.html) or
    [iRTcmix](../../iRTcmix/index-2.html)) with an attempt to identify
    the number of the line with the syntax error. Line numbers are
    cumulative, however, so for embedded apps the reported line may be
    radically different from the line in the sent scorefile.  
      

  - The original **Minc** parser was coded by Lars Graf. Additional
    modifications were made by John Gibson, Doug Scott, and others.

  

-----

  
The following is from the original cmix **Minc** documentation. Some is
a bit dated, but the basic concepts are still valid. For a more complete
and current grammar, see the RTcmix/src/parser/minc/minc.y file:

