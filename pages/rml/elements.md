---
layout: page
title: Elements
parent: rml
---

{{page.lib_name}} has no prior understanding of XML elements other than the built-in [element types](element_index.html).

When parsing a tag {{page.lib_name}} will look up custom elements associated with the tag name and fall back to a generic element. Very few custom elements are required as most of the power in {{page.lib_name}} comes from styling elements with [RCSS](../rcss.html) to produce the desired layout.

The user is encouraged to follow standard HTML guidelines where possible to improve readability across the community. For more information on setting up {{page.lib_name}} to emulate HTML4 see [Appendix: HTML4 Style Sheet](html4_style_sheet.html).

### Global Attributes

Elements have a base set of attributes that are common to across all types, which are:

`id`{:.attr} = id (CS)
: The unique identifier for the element in this document.

`class`{:.attr} = cdata (CI)
: Assigns a class name or set of class names to an element. Any number of elements may be assigned the same class name or names. Multiple class names must be separated by white space characters. See [RCSS](../rcss.html).

`style`{:.attr} = cdata (CS)
: Specifies inline style information for the element. See [RCSS](../rcss.html).

`on*`{:.attr} events = cdata (CS)
: Event bindings. See [Events](events.html).

