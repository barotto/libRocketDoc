---
layout: page
title: Building Boost::Python
---

This page describes how to build Boost::Python for use with {{page.lib_name}}. This describes how to build against a debug version of Python on the Windows platform.

### Directory Structure

The default {{page.lib_name}} configurations assume there is a support folder at the same directory structure level as the main rocket folder.

```
D:\development
  rocket
    Include
    Source
    Build
    ...
  support
    boost
    freetype
    python
    lib
```

### Building Python

This is straight forward, get the latest source code for Python (Python 2.7 at the time of writing). Open the solution file under PC/VS8.0 (VS will ask you to update the solution).

Build a debug and release version of the python. Copy python27.dll and python27_d.dll into support/lib directory.

### Build Boost::Python

Boost uses its own custom bjam build system. Download boost (1.44 at the time of writing) and a prebuilt copy of bjam.exe. Next we need to create a custom bjam-config.jam file that tells bjam where our copy of python is. This file needs to be placed in your home directory (**echo %HOMEDRIVE%%HOMEPATH%** in a command prompt will give you this path. Its C:\\Users\\\<username\>\\ on Windows Vista/7)

```
using python : 2.7 : D:\\development\\support\\python\\pcbuild\\python ;
using python : 2.7 : D:\\development\\support\\python\\pcbuild\\python_d
  : # includes
  : # libs                            
  : <python-debugging>on ;
```

Unfortunately the includes path above only supports one directory, but on PC python requires two include paths include and pc. To get around this, we use the visual studio environment variables.

```
D:\Development\support\boost\libs\python\build>set CL=/ID:\Development\support\python\include /ID:\Development\support\python\pc
D:\Development\support\boost\libs\python\build>set LIB=D:\Development\support\python\pc\vs8.0
```

We then set bjam to work building us debug and release versions.

```
D:\Development\support\boost\libs\python\build>bjam --toolset=msvc python-debugging=on variant=debug
D:\Development\support\boost\libs\python\build>bjam --toolset=msvc variant=release
```

Copy the lib and dll files from the boost/bin.v2 folder to the support/lib folder

You've now built all the required libraries and put them in the correct locations. RocketPython should now build correctly. You'll most likely need to copy all the dll files into the folder you run your application from as well.
