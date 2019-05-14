---
layout: page
title: User interface
parent: rcss
---

### Cursors: the 'cursor' property

`cursor`{:.prop}

Value: | \<string\> \| auto
Initial: | auto
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

This property defines the cursor to display while the mouse is hovering over the element. Values have the following meanings:

`auto`{:.value}
: The default cursor is used. 

`<string>`{:.value}
: The RML document loaded as a cursor and with \<string\> as a title is used as a cursor. If none exists, the default cursor is used. 

Cursors can be loaded into {{page.lib_name}} using the `{{page.lib_ns}}::Core::Context::LoadCursor()` function.

### Drag & drop: the 'drag' property

The `drag`{:.prop} property is a new property, not shared by CSS. It controls the generation of events as the mouse cursor begins a drag over an element (ie, clicks the left mouse button and moves the mouse), drags over elements (moves the mouse with the button depressed) and finishes a drag, or 'drops', an element over another.

`drag`{:.prop}

Value: | none \| drag \| drag-drop \| block
Initial: | none
Applies to: | all elements
Inherited: | no
Percentages: | N/A

Values have the following meanings:

`none`{:.value}
: If the mouse begins a drag over this element, the element is not dragged and does not generate any drag messages. However, it does not prevent an ancestor element from being dragged. 

`drag`{:.value}
: If the mouse begins a drag over this element, the element will begin spawning drag messages. However, no messages will be directed to other elements relating to the drag or the eventual drop. 

`drag-drop`{:.value}
: If the mouse begins a drag over this element, the element will begin spawning drag messages. Messages will also be directed to over elements if the dragged element is dragged over them. 

`block`{:.value}
: If the mouse begins a drag over this element, the element is not dragged and neither is any ancestor element. 

### Tab index: the 'tab-index' property

The 'tab-index' property is introduced for RCSS. It controls the generation of a tab order, which is an ordered list of elements within a document that gain input focus in turn as the 'tab' key is pressed.

`tab-index`{:.prop}

Value: | none \| auto
Initial: | none
Applies to: | all elements
Inherited: | no
Percentages: | N/A

Values have the following meanings:

`none`{:.value}
: The element is not part of the tabbing order. 

`auto`{:.value}
: The element inserts itself into the tabbing order in a location relative to its order in the element hierarchy. 
