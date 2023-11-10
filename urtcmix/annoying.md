---
title: uRTcmix on Catalina (10.15.x) or later
layout: ref
---

# uRTcmix on Catalina (10.15.x) or later

If OSX is complaining that uRTcmix isn't allowed to run because it isn't
from one of their 'signed developers', the following should set the
permissions so it can work:

    1.  start up a Terminal window (/Applications/Utilities/Terminal.app)

    2.  type in:

          "sudo xattr -rd com.apple.quarantine "

    without the quotes (and note the space at the end -- it has to be there)

    3. Drag/Drop the uRTcmix folder that should be in your Unity project
    "Assets" folder into
    the Terminal window with the "sudo ..." command typed in.  The result
    should be a line in the Terminal window that looks something like this:

          sudo xattr -rd com.apple.quarantine /Users/whoever/myUnityproject/Assets/uRTcmix

    4.  Hit &ltreturn>, give your password, and hopefully(!) all will be happy.

Unfortunately you will have to do this for every Unity project that uses
uRTcmix.  
  
Brad Garton  
January, 2021
