---
layout: page
title: Controls plugin
parent: cpp_manual
---

### Initialisation

To use the Controls plugin, include the main header Rocket/Controls.h and call the Rocket::Controls::Initialise() function after Rocket itself has been initialised. This will set up the instancers for the custom elements in the Controls package.

### Element packages

The Controls plugin includes three element packages:

* [Form controls](controls/form.md); a comprehensive set of form controls.
* [Data grids](controls/data_grid.md); elements for positioning and rendering dynamic, tabulated data.
* [Tab sets](controls/tab_set.md); elements for partitioning content into panels. 