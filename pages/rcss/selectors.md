---
layout: page
title: Selectors
description: Selectors
---

Selectors are used to select elements to apply specific rules to. The selector syntax supported in RCSS is as follows:

Pattern | Meaning
--- | ---
E | Matches any element of type E (ie, an element declared in an RML document as <E>).
E F | Matches any element of type F that is a descendant of an E element.
E.foo | Matches any element of type E that has been declared with class 'foo'.
E#foo | Matches any element of type E that has been declared with an ID of 'foo'.
E:foo | Matches any element of type E that has the pseudo-class 'foo' currently active.

Note that attribute selectors, the '+' selector, and the '>' selector are currently not supported.

The following pseudo-classes are supported natively (this includes the full range of proposed structural selectors from CSS3):

Pseudo-class | Meaning
--- | ---
:hover | Matches an element that is currently under the mouse cursor.
:active | Matches an element that has been clicked on, only until the button is released.
:focus | Matches an element that has input focus.
:nth-child(an + b) | Matches an element that has an + b - 1 siblings before it.
:nth-last-child(an + b) | Similar to nth-child, but counts backwards.
:nth-of-type(an + b) | Similar to nth-child, but only counts sibling elements of the same type.
:nth-last-of-type(an + b) | Similar to nth-of-type, but counts backwards.
:first-child | Matches an element that is the first child of its parent.
:last-child | Matches an element that is the last child of its parent.
:first-of-type | Matches an element that is the first child of its type.
:last-of-type | Matches an element that is the last child of its type.
:only-child | Matches an element that has no sibling elements.
:only-of-type | Matches an element that has no sibling elements of its type.
:empty | Matches an element that has no child nodes.

For a much fuller description of the :nth- style selectors, please refer to the respective page of the CSS3 working draft. 'even' and 'odd' are supported in RCSS.

A simple selector is made up of an optional element type followed by zero or more class selectors, ID selectors and pseudo-class selectors. If an element type is not given, any element type will be matched. So, for example, the selector:

```
div#level_list:hover
```

will match any element of type 'div' with an ID of 'level_list' that is currently being hovered by the cursor.

A selector is made up of potentially multiple simple selectors separated by whitespace. For an element to be matched by a selector with multiple simple selectors, it itself must match the last simple selector, and it must have descendants in the RML hierarchy that match, in order, each of the preceding simple selectors. So, for example, the selector:

```
div#level_list input.select option:nth-child(even)
```

will only match an element of type 'option' that is an even-numbered child of its parent, that has an ancestor of an 'input' element of class 'select', that itself has an ancestor that is a 'div' element with the ID 'level_list'.

Multiple selectors can be appended with commas, which is equivalent to an OR statement. For example, the following:

```
div#level_list,
div#weapon_list,
.colour_list
```

will match a 'div' element with ID 'level_list', or ID 'weapon_list', or any element that has a class of 'colour_list'.

See the CSS specifications for more thorough documentation and examples on the use of selectors. Please note that pseudo-classes (such as :first-letter, :before, etc) are not yet supported in RCSS. 