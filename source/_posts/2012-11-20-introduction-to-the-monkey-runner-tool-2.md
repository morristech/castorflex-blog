---
title: Introduction to the Monkey Runner tool
author: Antoine Merle
layout: post
comments: true
permalink: /introduction-to-the-monkey-runner-tool-2/
description: "Tutorial: first steps with monkey runner tool"
keywords: "android, tutorial, monkey, monkey runner, monkeyRunner, tool"
categories:
  - Tests
---
# 

Hi everyone welcome and in my blog for this post dedicated to Monkey Runner ([Introduction by Google here][1]). This post could be the extension of the first, which one was about Monkey ([link here][2]).   
  
This post won’t be a course but an introduction, because the aim is to discover by yourself how much this tool can be powerful. Monkey Runner is a tool which let the developer create programs which control the connected device/emulator.These programs will be developed in Python, and can install, launch and manipulate apps, and take screenshots. This is what we need to complete a Ant script which could build and sign the app for example. Indeed, if you have many applications, automate builds/tests, etc. becomes very interesting in order to gain time... and also because letting the computer to do all the job is pretty exciting!   
<!-- more -->  
Stop talking, and let’s go. The API is contained in three modules which are:

 [1]: http://developer.android.com/tools/help/monkeyrunner_concepts.html
 [2]: http://antoine-merle.com/monkey-tool/

*   [MonkeyRunner][3] : Class which provides methods for connecting a device/emulator.
*   [MonkeyDevice][4] : Class which provides methods for installing/uninstalling packages, sending events to an application (gestures, keyboard events, etc.)
*   [MonkeyImage][5] : Class which provides methods for capturing screenshots.

 [3]: http://developer.android.com/tools/help/MonkeyRunner.html "MonkeyRunner"
 [4]: http://developer.android.com/tools/help/MonkeyDevice.html "MonkeyDevice"
 [5]: http://developer.android.com/tools/help/MonkeyImage.html

Let’s create our monkeyrunner file, call it *mymonkey.py* for example (*.py* because I need my syntax coloring on vim :p)

```python
from com.android.monkeyrunner import MonkeyRunner, MonkeyDevice
import commands
import sys
    
# starting script
print "start"
     
# connection to the current device, and return a MonkeyDevice object
device = MonkeyRunner.waitForConnection()
     
apk_path = device.shell('pm path com.myapp')
if apk_path.startswith('package:'):
    print "myapp already installed."
else:
    print "myapp not installed, installing APKs..."
    device.installPackage('myapp.apk')
    
print "launching myapp..."
device.startActivity(component='com.myapp/com.myapp.MainActivity')
     
#screenshot
MonkeyRunner.sleep(1)
result = device.takeSnapshot()
result.writeToFile('./screenshots/splash.png','png')
print "screen 1 taken"
     
#sending an event which simulate a click on the menu button
device.press('KEYCODE_MENU', MonkeyDevice.DOWN_AND_UP)
     
print "end of script"
```
To launch this script you only have to execute this command:

`monkeyrunner mymonkey.py`

***Note 1***: *The monkeyrunner command is situated in [android-sdk-path]/tools/*   
  
***Note 2***: *If you are developing on windows, you may have to put the absolute paths (C:/etc.).*   
  
***Note 3***: *You can of course run your Unit tests !*  
  
And voilà! You have now the most basic script in the world of the universe, which connect monkeyrunner to a device, install and launch an application, take and save a screenshot, and clic on the menu button, you can now imagine all the things you can do.   
  
Please if you have any criticism/notes/suggestions, do not hesitate to write a comment, and if you liked this post, do not hesitate to share it!
