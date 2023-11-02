---
title: Standalone RTcmix
layout: ref
---

# Standalone RTcmix


-----

## Downloading RTcmix Source

The RTcmix source code lives on GitHub. There are two ways to download
it.

1.  Go to the [RTcmix Releases
    page](https://github.com/RTcmix/RTcmix/releases), and download the
    most recent (or other) release, using the "Source code (zip)" or
    "Source code (tar.gz)" buttons. Unpack this archive, if necessary,
    by double-clicking its icon. This should produce a folder called
    "RTcmix-5.2.2" (or a similar version number). Then move this folder
    into the place where you wish to keep the RTcmix source. The usual place
    for such things is /usr/local/src, or /opt/local/src, but it can be anywhere.  
      
    **-OR-**

2.  Assuming you are conversant with git, navigate to a directory where
    you would like the source code to live, and give this command in the
    shell:
    
        git clone https://github.com/RTcmix/RTcmix

## Configuring RTcmix 

In the RTcmix source code directory ("RTcmix/"), type the shell command:

``` 
./configure
```

There are options you may wish to use with the configure command for
your installation -- Perl, Python, X Windows, fftw-lib, etc. See the
INSTALL file in the top-level RTcmix source directory for a discussion of these.

## Compiling and Installing RTcmix 

After running the configure command (with any appropriate options), then
type (in that same "RTcmix/" directory):

``` 
make && make install
```

This will compile and install:

- The dynamic **RTcmix** library referenced by the main command-line tools
- The main command-line tool *CMIX*, plus *PCMIX* and *PYCMIX* if you have enabled Perl and/or Python support
- The complete set of loadable dynamic shared objects (DSOs) for the available instruments (see the [load](../reference/scorefile/load.html) command)
- A number of utility commands for examining and playing soundfiles, etc.
- On MacOS X, several helper apps used for visual display of some control functions

## Making the New Executables Visible to Your Shell

All **RTcmix** executable commands, including the *CMIX* command, are placed
in the "RTcmix/bin" directory. To make these commands visible to your shell, you can
copy/move them to a directory like "/usr/local/bin" or "/usr/bin". These
directories are probably already on your *command search path*. To see
your command search path, type the command:

``` 
echo $path
```

or

``` 
echo $PATH
```

and you should see a listing of all directories that are searched for
executable commands.

You can also add the "<PATH TO YOUR RTcmix DIRECTORY>/bin" directory to your command
search path. You will probably need to edit or create a ".tcshrc: or
".cshrc" or equivalent shell initialization file to do this. An example
of a line in a ".tcshrc" that accomplishes this is:

``` 
set path=(~/bin $path /usr/local/bin /usr/local/src/RTcmix/bin ".")
```

Once you do this, you will need to start a new Terminal or Shell window
(or use the unix *source* command) to reflect the change you have made.
See the documentation for "tcsh" or "csh" or "bash" for more
information.

If all else fails, you can type the whole pathname as a prefix to the
RTcmix executable command you want:

``` 
/usr/local/src/RTcmix/bin/CMIX
/usr/local/src/RTcmix/bin/sfprint somefile.aiff
/usr/local/src/RTcmix/bin/cpspch 8.07 
```
