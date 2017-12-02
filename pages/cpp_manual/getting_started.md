---
layout: page
title: Getting started with Rocket
description: Getting started with Rocket
---

This is the first place to look if you've just downloaded Rocket and want to kick the tyres.

### Samples

If you haven't already done so, take a look at the sample applications in /samples/. There you can find a whole heap of useful examples of how to use and abuse Rocket. Also in there you can find the shell library we used to develop all the samples (in /samples/shell/), which can be great if you want to write a quick and dirty application of your own.

### Setting up the build environment

Rocket is developed for use on the following platforms:

* Windows 32-bit compiling with Microsoft Visual Studio 2005-2010.
* MacOSX Intel 32/64bit compiling with GCC 4.
* Linux compiling with GCC 4. 

#### Visual Studio

* Add the Rocket include path (/include/ under the Rocket directory) to your include paths (either through Tools -> Options -> Projects and Solutions -> VC++ Directories -> Show directories for: Include files for all projects that you build, or Project -> Properties -> Configuration Properties -> C++ -> Additional Include Directories for this project only).
* #include <Rocket/Core.h> in your project.
* Add the Rocket library path (/bin/ under the Rocket directory) to your library paths.
* Link with RocketCore_d.lib (for debug builds) or RocketCore.lib (for non-debug builds).
* Copy the appropriate DLLs (ie, RocketCore_d.dll for debug builds, RocketCore.dll for non-debug builds) from the /bin/ folder into the directory your executable will run from. 

#### MacOSX / Linux

* Add the Rocket include path (/include/ under the Rocket directory) and library path (/bin/) to the paths in your build system.
* #include <Rocket/Core.h> in your project.
* Link with RocketCore.
* Either copy the Rocket libraries into your application's working directory, or set a LD_LIBRARY_PATH (DYLD_LIBRARY_PATH for MacOSX) environment variable. 

### Initialising Rocket

Before you can initialise Rocket, you'll need to set the interfaces that the library uses to interact with your application. There are two compulsory interfaces, the system interface and the render interface.

#### The system interface

The system interface is defined in <Rocket/Core/SystemInterface.h>. In order to create a valid system interface, you'll need to create a class that inherits from Rocket::Core::SystemInterface and provides the function:

```cpp
virtual float GetElapsedTime();
```

The function should return the time (in seconds) since the start of the application. Install your system interface by calling Rocket::Core::SetSystemInterface() with a pointer to the interface. Note that Rocket won't release your interfaces!

For more uses of the system interface, see the [documentation](interfaces.html#the-system-interface).

#### The render interface

The render interface is defined in <Rocket/Core/RenderInterface.h>. It provides a way for Rocket to send its geometry into your application's rendering pipeline. If you want to get Rocket up and running as quickly as possible in your own application, you can copy the render interface defined in the sample shell if your application is using OpenGL (you can find this at /samples/shell/include/ShellRenderInterface.h and /samples/shell/src/ShellRenderInterface.cpp), or the DirectX sample if your application is using DirectX 9 (you can find this at /samples/basic/directx/src/RenderInterfaceDirectX.*).

One you have a render interface for your application, install it into Rocket by calling Rocket::Core::SetRenderInterface().

If you'd like to take an in-depth look at setting up your own render interface, please see the [documentation](interfaces.html#the-render-interface).

#### Initialising the library

Call Rocket::Core::Initialise() once you have installed the system and render interfaces and Rocket will start up.
NOTE: If you have licensed rocket, please pass your license key to the initialise function.

### Creating a context

All elements within Rocket are part of a context. You must have at least one context in order to load, manipulate and render and interface elements. To create a context, use the Rocket::Core::CreateContext() function, passing in the name of the new context and its initial dimensions like so:

```cpp
Rocket::Core::Context* context = Rocket::Core::CreateContext("default", Rocket::Core::Vector2i(1024, 768));
```

You can release the context when you're done with it by calling RemoveReference().

#### Updating and rendering

Your application will need to update and render each context it maintains, as appropriate. Call the Update() function on each context as often as is necessary to update the context (usually after the frame's input has been injected), and Render() at the appropriate place in your application's render loop.

### Loading a document

Once you have a valid context, you can load a document into the context with the LoadDocument() function. LoadDocument() takes a single parameter, a string with the document's file name. If the load is successful you'll get a pointer to a Rocket::Core::ElementDocument back; call Show() on the document to make it visible.

```cpp
Rocket::Core::ElementDocument* document = context->LoadDocument("../../assets/demo.rml");
if (document != NULL)
	document->Show();
```

Unload the document by calling UnloadDocument() on its parent context.

```cpp
document->GetContext()->UnloadDocument(document);
```

### Injecting input

Once you've got a document loading and rendering, the next step is to get your input into Rocket. The context object has a range of functions for sending mouse, keyboard and text input into the system:

```cpp
// Sends a key down event into this context.
void ProcessKeyDown(Rocket::Core::Input::KeyIdentifier key_identifier, int key_modifier_state);
// Sends a key up event into this context.
void ProcessKeyUp(Rocket::Core::Input::KeyIdentifier key_identifier, int key_modifier_state);

// Sends a single character of text as text input into this context.
void ProcessTextInput(Rocket::Core::word character);
// Sends a string of text as text input into this context.
void ProcessTextInput(const Rocket::Core::String& string);

// Sends a mouse movement event into this context.
void ProcessMouseMove(int x, int y, int key_modifier_state);
// Sends a mouse-button down event into this context.
void ProcessMouseButtonDown(int button_index, int key_modifier_state);
// Sends a mouse-button up event into this context.
void ProcessMouseButtonUp(int button_index, int key_modifier_state);
// Sends a mouse-wheel movement event into this context.
void ProcessMouseWheel(int wheel_delta, int key_modifier_state);
```

Call the appropriate input functions to inject all relevant user input into your Rocket context each frame, before you call Update(). Note that Rocket does not translate key presses into text; this is up to the application. For more information, see the chapter on user input.

### Where next?

Now that you've had a (very!) brief introduction to Rocket, it is recommended you read the [packages guide](packages.html) and the [object overview](core_overview.html) to get an understanding of the composition of Rocket. From there, either work your way through the documentation, or dive on into the code and consult it as necessary. 
