---
layout: page
title: Events
description: Events
---

Events are sent to elements to indicate actions that have occurred to that element. Rocket generates many events internally (these are fully specified in the [RML event documentation](../rml/events.html)). The application can also send arbitrary events to elements.

When an event is dispatched to an element, it first goes through a bubble phase where the each of the element's ancestors has an opportunity to process the event and stop the propagation. The event is then sent to the target element, then goes through a capture phase where it falls back to its root ancestor.

Events are identified by a descriptive string name (such as "keydown", "blur", etc) and a dictionary of parameters that further describe the event. For example, the "keydown" event has parameters for identifying the actual key that was pressed and the state of the key modifiers.

Events can be handled internally by the elements they are sent to or by an event listener object. An event listener is able to subscribe to specific events on an element and will be notified whenever those events occur.

### Event interface

An event is represented by the Rocket::Core::Event structure, defined in Rocket/Core/Event.h. The public interface to an event object is:

```cpp
class Event
{
public:
	enum EventPhase { PHASE_UNKNOWN, PHASE_CAPTURE, PHASE_TARGET, PHASE_BUBBLE };

	// Get the current propagation phase.
	// @return Current phase the event is in.
	Rocket::Core::Event::EventPhase GetPhase() const;

	// Get the current element in the propagation.
	// @return The current element in propagation.
	Rocket::Core::Element* GetCurrentElement() const;

	// Get the target element.
	// @return The target element of this event.
	Rocket::Core::Element* GetTargetElement() const;

	// Get the event type.
	// @return The event type.
	const EMP::Core::String& GetType() const;

	// Returns the value of one of the event's parameters.
	// @param key[in] The name of the desired parameter.
	// @return The value of the requested parameter.
	template < typename T >
	T GetParameter(const EMP::Core::String& key, const T& default_value);

	// Stops the propagation of the event.
	void StopPropagation();
};
```

The phase of the event, returned by GetPhase(), will be PHASE_CAPTURE if the event listener received the event during the capture phase or PHASE_BUBBLE if during the bubbling phase.

The target element, returned by GetTargetElement(), is the element the event was originally sent to. The current element, returned by GetCurrentElement(), is the element the event is currently being sent to. This may be the target element or one of the target element's ancestors.

The name of the event ("keydown", "focus", etc) is returned from GetType(). You can also use the equality operator to compare an event directly with a string.

You can fetch the parameters of the event with the templated GetParameter() function. The exact parameters of each event are detailed in the [event documentation](../rml/events.html).

For event types that can be interrupted, a listener can call the StopPropagation() function on an event to prevent the event's progress through the event cycle.

### Event listeners

Any object that wants to listen for events derives from Rocket::Core::EventListener, and implements the one required pure virtual function:

```cpp
// Process the incoming event.
virtual void ProcessEvent(Rocket::Core::Event& event) = 0;
```

The ProcessEvent() function will be called every time a relevant event is sent to an element the listener is subscribed to.

#### Attaching to an element

To subscribe an event listener to an element, call the AddEventListener() function on the element to attach to.

```cpp
// Adds an event listener to this element.
// @param[in] event Event to attach to.
// @param[in] listener The listener object to be attached.
// @param[in] in_capture_phase True to attach in the capture phase, false in bubble phase.
void AddEventListener(const EMP::Core::String& event,
                      Rocket::Core::EventListener* listener,
                      bool in_capture_phase = false);
```

The function takes the following parameters:

* event: The string name of the event the listener wants to attach to, for example "keydown", "focus", etc.
* listener: The event listener object to attach.
* in_capture_phase: If true, the event listener will receive the event in the capture phase, otherwise, in the bubbling phase. See the RML event documentation for more information. 

#### Detaching from an element

To unsubscribe an event listener from an element, call the RemoveEventListener() function on the element:

// Removes an event listener from this element.
// @param[in] event Event to detach from.
// @param[in] listener The listener object to be detached.
// @param[in] in_capture_phase True to detach from the capture phase, false from the bubble phase.
void RemoveEventListener(const EMP::Core::String& event,
                         Rocket::Core::EventListener* listener,
                         bool in_capture_phase = false);

### Sending events

The application can send an arbitrary event to an element through the DispatchEvent() function on Rocket::Core::Element.

```cpp
// Sends an event to this element.
// @param[in] event Name of the event in string form.
// @param[in] parameters The event parameters.
// @param[in] interruptible True if the propagation of the event be stopped.
void DispatchEvent(const EMP::Core::String& event,
                   const EMP::Core::Dictionary& parameters,
                   bool interruptible = false);
```

The event will be created and sent through the standard event loop. The following example sends a "close" event to an element:

```cpp
EMP::Core::Dictionary parameters;
parameters.Set("source", "user");

element->DispatchEvent("close", parameters);
```

### Custom events

Events are instanced through an event instancer similarly to contexts. The instancer can be overridden with a custom instancer if a custom event is required; this is generally only needed to integrate a scripting language into Rocket.

A custom event inherits from Rocket::Core::Event. There are no virtual functions to be overridden.

#### Creating a custom event instancer

A custom event instancer needs to be created and registered with the Rocket factory in order to have custom events instanced. A custom event instancer derives from Rocket::Core::EventInstancer and implements the required pure virtual functions:

```cpp
// Instance an event object.
// @param[in] target Target element of this event.
// @param[in] name Name of this event.
// @param[in] parameters Additional parameters for this event.
// @param[in] interruptible If the event propagation can be stopped.
virtual Rocket::Core::Event* InstanceEvent(Rocket::Core::Element* target,
                                           const EMP::Core::String& name,
                                           const EMP::Core::Dictionary& parameters,
                                           bool interruptible) = 0;

// Releases an event instanced by this instancer.
// @param[in] event The event to release.
virtual void ReleaseEvent(Event* event) = 0;

// Releases this event instancer.
virtual void Release() = 0;
```

InstanceEvent() will be called whenever the factory is called upon to instance an event. The parameters to the function are:

* target: The element the event is begin targeted at.
* name: The name of the event ("keydown", "focus", etc).
* parameters: The parameters to the event as a dictionary.
* interruptible: True if the event can be interrupted (ie, prevented from propagating throughout the entire event cycle), false if not. 

If InstanceEvent() is successful, return the new event. Otherwise, return NULL (0) to indicate an instancing error.

ReleaseEvent() will be called when an event instanced through the instancer is no longer required by the system. It should be deleted appropriately.

Release() will be called when the event instancer is no longer required, usually when Rocket is shut down. The instancer should delete itself as appropriate.

#### Registering an instancer

To register a custom instancer with Rocket, call the RegisterEventInstancer() function on the Rocket factory (Rocket::Core::Factory) after Rocket has been initialised.

```cpp
// Registers an instancer for all events.
// @param[in] instancer The instancer to be called.
// @return The registered instanced on success, NULL on failure.
static Rocket::Core::EventInstancer* RegisterEventInstancer(Rocket::Core::EventInstancer* instancer);
```

Like other instancers, the event instancer is reference counted. Remember to remove the initial reference after you register a custom instancer with the factory.

### Inline events

Event responses can be specified as element attributes inside RML, similarly to HTML. For example, in the following RML fragment a response is given to the "click" event.

```
<rml>
	<head>
	</head>
	<body>
		<button onclick="game.start()">Start Game</button>
...
```

Notice the "on" prefix before the event name of "click". All event bindings from RML are prefixed this way.

Rocket sends inline events to event listener proxy objects that are created by the application. An application must therefore register a custom event listener instancer to have an opportunity to interpret the events.
Creating a custom event listener instancer

A custom event listener instancer derives from Rocket::Core::EventListenerInstancer. The following pure virtual functions must be implemented:

```cpp
// Instance an event listener object.
// @param value Value of the event.
virtual Rocket::Core::EventListener* InstanceEventListener(const EMP::Core::String& value) = 0;

// Releases this event listener instancer.
virtual void Release() = 0;
```

InstanceEventListener() will be called during RML parsing whenever the factory needs to find an event listener for an inline event. The parameter value will be the raw event response string as specified in the RML.

Release() will be called when the event instancer is no longer required, usually when Rocket is shut down. The instancer should delete itself as appropriate. 