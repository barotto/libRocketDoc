---
layout: page
title: RML Forms
parent: rml
---

A form is a collection of input elements inside a `<form>`{:.tag} element - when the form is "submitted", the information stored in the each child input element is sent through to the listening process.

Form elements are supported only when the [Controls plugin]({{"pages/cpp_manual/controls.html"|relative_url}}) is used.

### \<form\>

_Attributes_

`onsubmit`{:.attr} = cdata (CI)
: The name of the event to trigger when the form is submitted. The values of all form element children are passed to the event as parameters.

#### \<form\> Children

There are four elements that function as children to a `<form>`{:.tag} element: the `<input>`{:.tag}, `<textarea>`{:.tag}, `<select>`{:.tag} and `<dataselect>`{:.tag} elements. These have many common attributes which are listed below. Any specific attributes are listed afterwards under their own headings.

_Attributes_

`name`{:.attr} = cdata (CI)
: The name of the input element. This is used to look up the element's value when the form is submitted. For types of radio it is used to group together radio buttons so that only one with the same name can be checked at once.

`value`{:.attr} = cdata (CN)
: For types of `text`{:.value}, `password`{:.value} and `range`{:.value}, this is used as the initial value of the element. For the `radio`{:.value} and `checkbox`{:.value} types this is used as the value that is submitted if the input is checked. For `<option>`{:.tag} elements, this is the value that is submitted by the parent `<select>`{:.tag}, if it is the selected option.

`disabled`{:.attr} (CI)
: If this attribute is set, then the input element is unable to be changed by user input.

#### \<input\>

_Attributes_

`type`{:.attr} = cdata (CI)
: The type of the input field. Must be one of:
* `text`{:.value} - A one-line text-entry field.
* `password`{:.value} - Like text, but replaces the entered text with asterisks.
* `radio`{:.value} - A radio button.
* `checkbox`{:.value} - A checkbox.
* `range`{:.value} - A slider bar.
* `submit`{:.value} - A button for submitting the form. 

##### Text and Password types

`size`{:.attr} = number (CN)
: For types of text and password, defines the length (in characters) of the element.

`maxlength`{:.attr} = number (CN)
: For types of text and password, defines the maximum length (in characters) that the element will accept.

##### Radio and Checkbox types

`checked`{:.attr} (CI)
: For types of radio and checkbox, if this attribute is set then the element is "on".

##### Range type

`min`{:.attr} = number (CN)
: For the range type, defines the value at the lowest (left or top) end of the slider.

`max`{:.attr} = number (CN)
: For the range type, defines the value at the highest (right or bottom) end of the slider.

`step`{:.attr} = number (CN)
: For the range type, defines the increment that the slider will move by.

`orientation`{:.attr} = cdata (CI)
: For the range type, specifies if it is a vertical or horizontal slider. Values can be `horizontal`{:.value} or `vertical`{:.value}.

#### \<textarea\>

_Attributes_

`cols`{:.attr} = number (CN)
: The width of the visible area of the textarea, as a number of columns of text.

`rows`{:.attr} = number (CN)
: The height of the visible area of the textarea, as a number of text rows.

`wrap`{:.attr} = cdata (CI)
: If set `nowrap`{:.value}, the textarea will not wrap unbroken lines to a new row.

`maxlength`{:.attr} = number (CN)
: The maximum length (in characters) that the element will accept.

#### \<select\>

`<select>`{:.tag} has no additional parameters.

##### \<option\>

_Attributes_

`selected`{:.attr} = cdata (CI)
: If set, then the option is selected when the `<select>`{:.tag} element is first loaded.

`unselectable`{:.attr} = cdata (CI)
: If set, then the option is not selectable by the user. Useful for labelling groups of options.

#### \<dataselect\>

_Attributes_

`source`{:.attr} = datasource (CS)
: The data source to read the options from.

`fields`{:.attr} = cdata (CS)
: A comma-separated list of fields to fetch from the data source and to display for each option (or optionally to be sent through the data formatter)

`valuefield`{:.attr} = cdata (CS)
: The field from the data source to use as the option's value. If not set, then the first field in the fields attribute is used.

`formatter`{:.attr} = cdata (CS)
: The name of the dataformatter to use to process the raw fields information into RML. If not set, then the fields are displayed in raw text.
