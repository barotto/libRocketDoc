---
layout: page
title: Overview
parent: python_manual
---

RocketPython can be used in an application that extends or embeds Python. The only limitation is that your application initialises {{page.lib_name}} with the necessary System and Render interfaces, but these can easily be done from a custom Python module.

To start using RocketPython simply import the module as you would any other extension. RocketPython will then plug itself into the running {{page.lib_name}} instance. It is important to import RocketPython before you create any {{page.lib_name}} Contexts, Documents or Elements otherwise you will not have access to these items from Python.

For a full list of accessable classes and methods please see the {{page.lib_name}} Python [API Reference](api_reference.html).

### Requirements

RocketPython requires [Python 2.5](http://www.python.org) and [Boost::Python 1.35](http://www.boost.org).

*NOTE: On the Windows platform rocket assumes you will be compiling against the debug python libraries. All other platforms expect the release version.*

### Getting Started

**Windows**  
You can download our pre-built support package which includes both of these libraries. Extract the archive into C:\rocket_support and the projects should compile without modification.

**macOS / Linux**  
You'll need to compile and install both libraries and then update config.py (in the build directory) with the relevant paths before attempting to compile any python related code. See README.TXT in the samples folder for more information.

The best place to start exploring RocketPython is to look at the PyInvaders sample application which is a rework of the standard Invaders sample but with python support. There is also a step by step [tutorial](../tutorials/python_event_system.html) describing the steps taken to do the conversion.
