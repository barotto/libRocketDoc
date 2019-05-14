---
layout: page
title: User input
parent: cpp_manual
---

{{page.lib_name}} does not read user input, but requires the application to feed its contexts with input events. Each context will process the input it is provided with and dispatch events as appropriate.

### Key modifiers

Most of the input functions take the parameter `key_modifier_state`. This is a bitmask of active key modifiers; keys such as Control, Alt, etc, as well as the lock keys. This is used to generate the key modifier parameters on any events that are spawned, so is entirely optional. If you don't want or need the key modifier parameters on your input events, feel free to pass `0`{:.value} for the `key_modifier_state` into all the input functions you call.

The bitmask should be configured using the enumeration `{{page.lib_ns}}::Core::Input::KeyModifier`, detailed below:

```cpp
enum KeyModifier
{
	KM_CTRL = 1 << 0,	// Set if at least one Ctrl key is depressed.
	KM_SHIFT = 1 << 1,	// Set if at least one Shift key is depressed.
	KM_ALT = 1 << 2,	// Set if at least one Alt key is depressed.
	KM_META = 1 << 3,	// Set if at least one Meta key (the command key) is depressed.
	KM_CAPSLOCK = 1 << 4,	// Set if caps lock is enabled.
	KM_NUMLOCK = 1 << 5,	// Set if num lock is enabled.
	KM_SCROLLLOCK = 1 << 6	// Set if scroll lock is enabled.
};
```

See the documentation on [RML events](../rml/events.html#events) for a specification of the key modifier events parameters.

### Mouse input

Different aspects of mouse input are given to a context through a variety of functions, detailed below.

#### Mouse movement

Call the `ProcessMouseMove()` function on a context to inform the context that the position of the mouse cursor within the context has changed.

```cpp
// Sends a mouse movement event into this context.
// @param[in] x The x-coordinate of the mouse cursor.
// @param[in] y The y-coordinate of the mouse cursor.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessMouseMove(int x, int y, int key_modifier_state);
```

Note that the x and y coordinates are in pixel offsets from the top-left of the context. If the position of the mouse cursor is not different from the last time `ProcessMouseMove()` was called, no action will be taken. If the mouse has moved, then any of the following events may be generated, targeted at the appropriate elements:

* `onmousemove`{:.evt}
* `onmouseover`{:.evt}
* `onmouseout`{:.evt}
* `ondragstart`{:.evt}
* `ondrag`{:.evt}
* `ondragover`{:.evt}
* `ondragout`{:.evt}

#### Mouse buttons

Call `ProcessMouseButtonDown()` and `ProcessMouseButtonUp()` on a context to to inform the context when a mouse button is pressed or released.

```cpp
// Sends a mouse-button down event into this context.
// @param[in] button_index The index of the button that was pressed; 0 for the left button, 1 for right, and any others from 2 onwards.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessMouseButtonDown(int button_index, int key_modifier_state);

// Sends a mouse-button up event into this context.
// @param[in] button_index The index of the button that was release; 0 for the left button, 1 for right, and any others from 2 onwards.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessMouseButtonUp(int button_index, int key_modifier_state);
```

`ProcessMouseButtonDown()` may generate any of the following events:

* `onfocus`{:.evt}
* `onblur`{:.evt}
* `onmousedown`{:.evt}

`ProcessMouseButtonUp()` may generate:

* `onmouseup`{:.evt}
* `onclick`{:.evt}
* `ondblclick`{:.evt}
* `ondragdrop`{:.evt}
* `ondragend`{:.evt}

#### Mouse wheel

If you want to send mouse-wheel events to your documents, call the `ProcessMouseWheel()` function on your contexts as appropriate.

```cpp
// Sends a mouse-wheel movement event into this context.
// @param[in] wheel_delta The mouse-wheel movement this frame. {{page.lib_name}} treats a negative delta as up movement (away from the user), positive as down.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessMouseWheel(int wheel_delta, int key_modifier_state);
```

`ProcessMouseWheel()` will generate an `onmousescroll`{:.evt} event targeted at the hover element. By default, all elements will use this event to scroll their contents up and down if appropriate.

### Key input

The key input functions use the `KeyIdentifier` enumeration found in `<{{page.lib_dir}}/Core/Input.h>`{:.incl}; refer to that file for the possible values. They are modeled after the Windows virtual key codes (the VK_* enumeration), so should be familiar to Windows developers. Any confusing enumeration names are explained in the comments.

{{page.lib_name}} makes a distinction between key input and text input; key input (specified by the `ProcessKeyDown()` and `ProcessKeyUp()` functions) refers to actual physical key presses, while text input refers to characters being generated from user input. Depending on user locale, it may take more than one physical key stroke to generate a single character of text input. At present, {{page.lib_name}} offers no translation between key input and text input; that is left up to the application.

Call the following functions on a context to inform the context of key presses or releases:

```cpp
// Sends a key down event into this context.
// @param[in] key_identifier The key pressed.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessKeyDown({{page.lib_ns}}::Core::Input::KeyIdentifier key_identifier, int key_modifier_state);

// Sends a key up event into this context.
// @param[in] key_identifier The key released.
// @param[in] key_modifier_state The state of key modifiers.
void ProcessKeyUp({{page.lib_ns}}::Core::Input::KeyIdentifier key_identifier, int key_modifier_state);
```

`ProcessKeyDown()` will generate an `onkeydown`{:.evt} event targeted at the current focus element (if an element is in focus). `ProcessKeyUp()` will likewise generate the `onkeyup`{:.evt} event.

### Text input

{{page.lib_name}} takes text input as 16-bit, UCS2-encoded characters. When text input occurs that you wish a {{page.lib_name}} context to know about, use the following functions:

```cpp
// Sends a single character of text as text input into this context.
// @param[in] character The UCS-2 character to send into this context.
void ProcessTextInput({{page.lib_ns}}::Core::word character);

// Sends a string of text as text input into this context.
// @param[in] string The UCS-2 string to send into this context.
void ProcessTextInput(const {{page.lib_ns}}::Core::String& string);
```

These functions will generate an `ontextinput`{:.evt} event targeted at the context's current focus element (if there is one).

### Sample input processing

The sample shell (found under your {{page.lib_name}} installation at `/Samples/shell/`{:.path}) contains a sample implementation of input processing for all of {{page.lib_name}}'s supported platforms, including a key-to-text converter for a US-keyboard layout (see `/Samples/shell/src/Input.cpp`{:.path}). 