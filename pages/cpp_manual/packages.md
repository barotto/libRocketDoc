---
layout: page
title: Packages
description: Packages
---

Rocket ships with four libraries, all with full source code.

### RocketCore

RocketCore is the main Rocket library. It contains all the basic types, the basic element hierarchy, the style sheet and property system, the layout engine and the RML parser. RocketCore is all you need to display content from C++.

To use RocketCore, include <Rocket/Core.h> in your application and link with RocketCore or RocketCore_d.

### RocketControls

RocketControls contains custom elements for form controls (radio buttons, range sliders, etc), data tables and tabbed windows. You are free to use the plugin as-is in your application, or use the elements as a starting point for more specialised controls in your application.

This package is a great place to look at for examples of creating new elements and writing custom XML parsing.

To use RocketControls, include <Rocket/Controls.h> in your application and link with RocketControls or RocketControls_d.

This package also contains all the required code to expose the controls to Python if you are using Rocket with RocketPython.

### RocketDebugger

RocketDebugger is a visual debugger for Rocket elements, inspired by similar debuggers for Firefox. We strongly recommend you use this in your application during development!

To use RocketDebugger, include <Rocket/Debugger.h> in your application and link with RocketDebugger or RocketDebugger_d.

### RocketPython

RocketPython provides Python language bindings to RocketCore via a Python extension module. RocketPython greatly improves programmer productivity, via the power of scripting. It's what JavaScript is to the Web. 