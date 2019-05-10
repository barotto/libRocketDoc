---
layout: page
title: Planned Features
---

Based on discussions in the forums, the following are planned features for {{page.lib_name}}.

### Sooner

* Investigate switching the XML parser to RapidXML for speed
* Make the STL configurable (this is important for Custom Memory Allocators later too) 

### Later

* Add support to render a document to texture
* Send all allocs and frees through the SystemInterface, allowing developers to manage memory better
* Make contexts thread safe, so each context can run in its own thread without affecting the other 

### Some Day

* Address the global element transparency issues. Some elements don't currently support transparency, due to HTML spec, but I think its worth coming up with a way to allow this.
* Expose more of the font system, maybe making it a plugin, so that you're not required to use TrueType fonts.
* Allow text strings to be rendered at different angles, not just horizontal 

