---
layout: page
title: RML Forms
parent: rml
---

A form is a collection of input elements inside a FORM element - when the form is "submitted", the information stored in the each child input element is sent through to the listening process.

### FORM Element

*Attributes*

**onsubmit** = cdata (CI)
>The name of the event to trigger when the form is submitted. The values of all form element children are passed to the event as parameters.

#### FORM Element Children

There are four elements that function as children to a FORM element: the INPUT, TEXTAREA, SELECT and DATASELECT elements. These have many common attributes which are listed below. any specific attributes are listed afterwards under their own headings.

*Attributes*

**name** = cdata (CI)
>The name of the input element. This is used to look up the element's value when the form is submitted. For types of radio it is used to group together radio buttons so that only one with the same name can be checked at once.

**value** = cdata (CN)
>For types of text, password and range, this is used as the initial value of the element. For the radio and checkbox types this is used as the value that is submitted if the input is checked. For OPTION elements, this is the value that is submitted by the parent SELECT, if it is the selected option.

**disabled** (CI)
>If this attribute is set, then the input element is unable to be changed by user input.

#### INPUT Element

*Attributes*

**type** = cdata (CI)
>The type of the input field. Must be one of:
>
>* text - A one-line text-entry field.
>* password - Like text, but replaces the entered text with asterisks.
>* radio - A radio button.
>* checkbox - A checkbox.
>* range - A slider bar.
>* submit - A button for submitting the form. 

##### Text and Password types

**size** = number (CN)
>For types of text and password, defines the length (in characters) of the element.

**maxlength** = number (CN)
>For types of text and password, defines the maximum length (in characters) that the element will accept.

##### Radio and Checkbox types

**checked** (CI)
>For types of radio and checkbox, if this attribute is set then the element is "on".

##### Range type

**min** = number (CN)
>For the range type, defines the value at the lowest (left or top) end of the slider.

**max** = number (CN)
>For the range type, defines the value at the highest (right or bottom) end of the slider.

**step** = number (CN)
>For the range type, defines the increment that the slider will move by.

**orientation** = cdata (CI)

>For the range type, specifies if it is a vertical or horizontal slider. Values can be "horizontal" or "vertical".

#### TEXTAREA Element

*Attributes*

**cols** = number (CN)
>The width of the visible area of the textarea, as a number of columns of text.

**rows** = number (CN)
>The height of the visible area of the textarea, as a number of text rows.

**wrap** = cdata (CI)
>If set "nowrap", the textarea will not wrap unbroken lines to a new row.

**maxlength** = number (CN)
>The maximum length (in characters) that the element will accept.

#### SELECT Element

Select has no additional parameters.

##### OPTION Element

*Attributes*

**selected** = cdata (CI)
>If set, then the option is selected when the SELECT element is first loaded.

**unselectable** = cdata (CI)
>If set, then the option is not selectable by the user. Useful for labelling groups of options.

#### DATASELECT Element

*Attributes*

**source** = datasource (CS)
>The data source to read the options from.

**fields** = cdata (CS)
>A comma-separated list of fields to fetch from the data source and to display for each option (or optionally to be sent through the data formatter)

**valuefield** = cdata (CS)
>The field from the data source to use as the option's value. If not set, then the first field in the fields attribute is used.

**formatter** = cdata (CS)
>The name of the dataformatter to use to process the raw fields information into RML. If not set, then the fields are displayed in raw text.
