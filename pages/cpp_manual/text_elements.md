---
layout: page
title: Text Elements
parent: cpp_manual
---

Rocket uses text elements (elements derived from Rocket::Core::ElementText) to store and render loose text. Text elements are generated automatically for text in RML documents, and can be created dynamically by using the '#text' element instancer through the Rocket factory, or through the CreateTextNode() function on a document.

### Text encoding

The string type used through the majority of Rocket's interfaces, EMP::Core::String, stores 8-bit wide characters. In order to store wider characters efficiently, text elements store their contents as a Rocket::Core::String type. The Rocket string stores 16-bit wide characters in UCS-2 format; while not sufficient to store every possible UTF-8 character, it can store every useful one, so is a good compromise between size and usefulness.

When a Rocket string is constructed from an EMP string, it interprets the EMP string as being UTF-8 encoded and will convert it to UCS-2.

### HTML characters

Rocket text nodes support a subset of the full HTML-encoding for special characters to allow XML characters to be present in loose text. The characters supported are:

* `&lt;` The less-than symbol, '<'.
* `&gt;` The greater-than symbol, '>'.
* `&amp;` The ampersand symbol, '&'.
* `&nbsp;` A non-breaking space. 

You should use these symbols instead of their literal equivalents when putting them into RML. For example, the following RML fragment will most likely generate a parse error:

```
<p>You shouldn't use < or > characters in loose text.</p>
```

The following fragment puts the characters in correctly:

```
<p>You shouldn't use &lt; or &gt; characters in loose text.</p>
```

### Setting an element's text

The SetText() function on a Rocket::Core::ElementText will change the text on the text element to a new string.

```cpp
// Sets the raw string this text element contains.
// @param[in] text The new string to set on this element.
void SetText(const Rocket::Core::String& text);
```

If you pass in a constant string or an EMP::Core::String, it will be converted to a Rocket string as described above (interpreted as UTF-8 encoding). This means it will interpret standard ASCII characters OK.

Note that this sets the raw text on the element; the actual rendered text may differ due to whitespace processing.

### Retrieving an element's text

The GetText() function will return the element's raw text.

```cpp
// Returns the raw string this text element contains.
// @return This element's raw text.
const Rocket::Core::String& GetText() const;
```

This will return the raw text as a UCS-2 encoded Rocket string. To convert the string to a UTF-8 encoded string, call ToUTF8() on the string.

```cpp
EMP::Core::String utf8_string;
element_text->GetText().ToUTF8(utf8_string);
```

### String generation

Text elements are capable of generating formatted sub-sections of their content. This is generally only required by custom elements placing text internally; see the section on hidden elements for more information. 