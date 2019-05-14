---
layout: page
title: Selectors
parent: rcss
---

Selectors are used to select elements to apply specific rules to. The selector syntax supported in RCSS is as follows:

Pattern | Meaning
--- | ---
`E`{:.cls} | Matches any element of type E (ie, an element declared in an RML document as `<E>`{:.tag}).
`E F`{:.cls} | Matches any element of type F that is a descendant of an E element.
`E.foo`{:.cls} | Matches any element of type E that has been declared with class `foo`{:.cls}.
`E#foo`{:.cls} | Matches any element of type E that has been declared with an ID of `foo`{:.value}.
`E:foo`{:.cls} | Matches any element of type E that has the pseudo-class `foo`{:.cls} currently active.

Note that attribute selectors, the `+`{:.cls} selector, and the `>`{:.cls} selector are currently not supported.

The following pseudo-classes are supported natively (this includes the full range of proposed structural selectors from CSS3):

Pseudo-class | Meaning
--- | ---
`:hover`{:.cls} | Matches an element that is currently under the mouse cursor.
`:active`{:.cls} | Matches an element that has been clicked on, only until the button is released.
`:focus`{:.cls} | Matches an element that has input focus.
`:nth-child(an + b)`{:.cls} | Matches an element that has an + b - 1 siblings before it.
`:nth-last-child(an + b)`{:.cls} | Similar to nth-child, but counts backwards.
`:nth-of-type(an + b)`{:.cls} | Similar to nth-child, but only counts sibling elements of the same type.
`:nth-last-of-type(an + b)`{:.cls} | Similar to nth-of-type, but counts backwards.
`:first-child`{:.cls} | Matches an element that is the first child of its parent.
`:last-child`{:.cls} | Matches an element that is the last child of its parent.
`:first-of-type`{:.cls} | Matches an element that is the first child of its type.
`:last-of-type`{:.cls} | Matches an element that is the last child of its type.
`:only-child`{:.cls} | Matches an element that has no sibling elements.
`:only-of-type`{:.cls} | Matches an element that has no sibling elements of its type.
`:empty`{:.cls} | Matches an element that has no child nodes.

For a much fuller description of the `:nth-`{:.cls} style selectors, please refer to the respective page of the CSS3 working draft. `even`{:.cls} and `odd`{:.cls} are supported in RCSS.

A simple selector is made up of an optional element type followed by zero or more class selectors, ID selectors and pseudo-class selectors. If an element type is not given, any element type will be matched. So, for example, the selector:

```css
div#level_list:hover
```

will match any element of type `div`{:.tag} with an ID of `level_list`{:.value} that is currently being hovered by the cursor.

A selector is made up of potentially multiple simple selectors separated by whitespace. For an element to be matched by a selector with multiple simple selectors, it itself must match the last simple selector, and it must have descendants in the RML hierarchy that match, in order, each of the preceding simple selectors. So, for example, the selector:

```css
div#level_list input.select option:nth-child(even)
```

will only match an element of type `option`{:.tag} that is an even-numbered child of its parent, that has an ancestor of an `input`{:.tag} element of class `select`{:.cls}, that itself has an ancestor that is a `div`{:.tag} element with the ID `level_list`{:.value}.

Multiple selectors can be appended with commas, which is equivalent to an OR statement. For example, the following:

```css
div#level_list,
div#weapon_list,
.colour_list
```

will match a `div`{:.tag} element with ID `level_list`{:.value}, or ID `weapon_list`{:.value}, or any element that has a class of `colour_list`{:.value}.

See the CSS specifications for more thorough documentation and examples on the use of selectors. Please note that pseudo-classes (such as `:first-letter`{:.cls}, `:before`{:.cls}, etc) are not yet supported in RCSS. 