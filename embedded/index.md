---
title: Embedded RTcmix
layout: ref
---

# RTcmix as an Embedded Library


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

There are options you may wish to use with the configure command for your installation -- See the INSTALL file in the top-level RTcmix source directory for a discussion of these.

## Compiling the RTcmix Embedded Library for MacOS X

After running the configure command (with any appropriate options), then
type (in that same "RTcmix/" directory):

``` 
make BUILDTYPE=OSXEMBEDDED
```

## Compiling the RTcmix Embedded Library for Linux

After running the configure command (with any appropriate options), then
type (in that same "RTcmix/" directory):

``` 
make BUILDTYPE=LINUXEMBEDDED
```


These will compile the dynamic **RTcmix** library including the complete set of objects for the available instruments.  Running 'make install' will copy this library to the location that has been configured for it.

