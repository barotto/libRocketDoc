---
layout: page
title: Plugins
parent: cpp_manual
---

Rocket has a simple, straightforward system for writing plugins. Plugins receive notification when contexts and elements are created and destroyed.

### Creating a plugin

All plugins derive from the Rocket::Core::Plugin class. The virtual functions that can be overridden are:

```cpp
// Called when Rocket is initialised.
virtual void OnInitialise();
// Called when Rocket shuts down.
virtual void OnShutdown();

// Called when a document load request occurs, before the document's file is opened.
virtual void OnDocumentOpen(Context* context, const Rocket::Core::String& document_path);
// Called when a document is successfully loaded from file or instanced, initialised and added to its context. This is called before the document's 'load' event.
virtual void OnDocumentLoad(ElementDocument* document);
// Called when a document is unloaded from its context. This is called after the document's 'unload' event.
virtual void OnDocumentUnload(ElementDocument* document);

// Called when a new context is created.
virtual void OnContextCreate(Rocket::Core::Context* context);
// Called when a context is destroyed.
virtual void OnContextDestroy(Rocket::Core::Context* context);

// Called when a new element is created.
virtual void OnElementCreate(Rocket::Core::Element* element);
// Called when an element is destroyed.
virtual void OnElementDestroy(Rocket::Core::Element* element);
```

#### Rocket engine events

The OnInitialise() function will be called on all registered plugins when Rocket is successfully initialised. If Rocket is already initialised when a plugin is registered, OnInitialise() will be immediately called on the plugin.

OnShutdown() is called on all registered plugins when Rocket is shut down, immediately after all the contexts and elements are destroyed. Plugins must release any resources they have allocated, including themselves, during this call.

#### Document events

OnDocumentOpen is called when a RML stream is opened, Load/Unload are global callbacks called before and after the documents load/unload respectively.

#### Context events

OnContextCreate() and OnContextDestroy() are called on every registered plugin when a context is successfully created or destroyed.

#### Element events

OnElementCreate() and OnElementDestroy() are called on every registered plugin when an element is successfully created or destroyed.

### Registering a plugin

To register a plugin, call the RegisterPlugin() function in Rocket::Core.

```cpp
Rocket::Core::Plugin* plugin = new CustomPlugin();
Rocket::Core::RegisterPlugin(plugin);
```