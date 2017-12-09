---
layout: page
title: RML Events
parent: rml
---

The event system is based on the extremely flexible DOM event system.

When an event is fired at a target element, it bubbles up from the root to the target and then captures back down to root. Event listeners can be attached in either capture or bubble phase.

Binding done in RML will always attach to the bubble phase.

For a detailed description of the DOM event system see the W3C Specification.

### RML binding

Event listeners can be bound to an element declared in RML by specifying an attribute with the name of the event to bind to prefixed with 'on'. For example, to bind a listener to the click event, you would declare something like the following:

```html
<button onclick="load game">Start Game</button>
```

Note that this is the only time you prefix the event name with 'on'; all other times an event is referenced, it is done so simply with its name.

### Events in Python

When using the Python plugin, event listeners bound through RML execute Python code. The value of the event attribute will be executed when the event is triggered. Multiple lines of Python code can be put on one line, separated by a semicolon.

```html
<div onclick="print 'hello'; print 'goodbye'">
```

For full details on Python events see the [Python Manual](../python_manual.html).

### Events

Below is a list of events and their associated event attributes.

A number of input events send through key modifiers. In this case the following parameters are set to true if the key modifier is active when the action takes place:

* ctrl_key
* shift_key
* meta_key
* alt_key
* caps_lock_key
* num_lock_key
* scroll_lock_key 

#### General

**show**
>Sent to a document when it is made visible.

**hide**
>Sent to a document when it is made invisible.

**resize**
>Sent to an element when it resizes.

**scroll**
>Sent to an element when it is scrolled.

**focus**
>Sent to an element when it becomes the main focus.

**blur**
>Sent to an element when it has focus removed.


#### Keyboard Events

**keydown**
>Sent to the focus element when a key is pressed.
>* key_identifier: A value from the Rocket::Core::Input::KeyIdentifier enumeration (found in Rocket/Core/Input.h).
>* Key modifiers. 

**keyup**
>Sent to the focus element when a key is released.
>* key_identifier: A value from the Rocket::Core::Input::KeyIdentifier enumeration.
>* Key modifiers. 

**textinput**
>Send to the focus element when a text character is entered.
>* data: An Rocket::Core::word corresponding to the character value. 

#### Mouse Events

All mouse events send through key modifiers.

**click**
>Sent to the element under the mouse cursor when a mouse button is clicked. A click is defined as a button press followed by a button release over the same element.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* button: The button that was clicked. 

**dblclick**
>Sent to the element under the mouse cursor when a mouse button is double clicked. Note that the click event will always be sent before dblclick.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* button: The button that was clicked. 

**mouseover**
>Sent to an element as the mouse cursor moves onto it.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context. 

**mouseout**
>Sent to an element as the mouse cursor moves off it.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context. 

**mousemove**
>Sent to the element under the mouse cursor when the mouse is moved.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context. 

**mouseup**
>Sent to the element under the mouse cursor when a mouse button is released.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* button: The button that was released. 

**mousedown**
>Sent to the element under the mouse cursor when a mouse button is pressed down.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* button: The button that was pressed. 

**mousescroll**
>Sent to the focus element when the mouse's scroll wheel is scrolled.
>* wheel_delta: An integer value representing how far the wheel has been scrolled. 

#### Dragging

All mouse drag events send through key modifiers.

**dragstart**
>Sent to an element when it is first dragged, before the first drag event.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

**dragend**
>Sent to the dragged element when the mouse button is released.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

**drag**
>Sent to the dragged element when the mouse is moved.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

The following events are only sent if the dragged element has a drag property of drag-drop. They are sent to elements other than the dragged element, usually as the mouse moves across them.

**dragover**
>Sent during a drag operation to an element when the mouse cursor is moved onto the element (similarly to the mouseover event).
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

**dragout**
>Sent during a drag operation to an element when the mouse cursor is moved off the element (similarly to the mouseout event).
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

**dragmove**
>Sent during a drag operation to the top-most element being hovered over (excluding the dragged element) when the mouse is moved.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

**dragdrop**
>Sent at the end of a drag operation to the element the cursor is hovering over.
>* mouse_x: The mouse x position within the context.
>* mouse_y: The mouse y position within the context.
>* drag_element: The element that is being dragged. 

#### Forms

**submit**
>Sent to the form when it is submitted.
>* parameters: - The event object will have an attribute for each named value in the form. 

#### Form Controls

**change**
>Sent to a form control when its value is changed.
>* value: The new value. 

#### Documents

**load**
>Sent to a document when it is initially loaded. 

**unload**
>Sent to a document when it is unloaded. 

#### Handle

**handledrag**
>Sent to the handle when it is moved.
>* handle_x: Handle x position.
>* handle_y: Handle y position. 

#### DataGrid

**columnadd**
>Sent to the data grid when a column is added.
>* index: Index of the column that was added. 

**rowupdate**
>Sent to the data grid when any rows in the table are loaded. 

**rowadd**
>Sent to the data grid when rows are added to the table.
>* first_row_added Index of the first row added.
>* num_rows_added The number of rows added after the first row. 

**rowremove**
>Sent to the data grid when rows are removed from the table.
>* first_row_removed Index of the first row removed.
>* num_rows_removed The number of rows removed after the first row. 

#### TabSet

**tabchange**
>Sent to the tab set when the active tab is changed.
>* tab_index: The new tab index. 

