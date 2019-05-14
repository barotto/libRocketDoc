---
layout: page
title: Hidden elements
parent: cpp_manual
---

{{page.lib_name}} distinguishes between normal elements that are part of the DOM and visible to all subsystems, and hidden (or non-DOM) elements that (by default) can only be found if explicitly asked for. Hidden elements are typically used by custom elements; for example, the [drop-down select element](controls/form.html#drop-down-select-box) in the Controls plugin creates hidden elements for its arrow button, the value field and the selection box.

### Differences in hidden elements

The subsystems of {{page.lib_name}} that ignore hidden elements are:

* Automatic layout.
* RML serialisation; ie, `GetInnerRML()` will not generate RML for hidden elements. 

important subsystems that still recognise hidden elements are:

* Input events.
* Update and rendering.
* RCSS properties. 

Custom elements that make use of hidden elements can therefore control their size and positioning exactly, while still getting all the flexibility of the RCSS property system. Helper methods are made available for layout.

### Adding a hidden element

Hidden elements are created just like other elements, either through the [{{page.lib_name}} factory](elements.html#dynamically-creating-elements) or `CreateElement()` on a [document](documents.html#creating-new-elements).

To parent an element to another as a hidden element, call `AppendChild()` as normal but set the second parameter to `false`.

```cpp
// Append a child to this element.
// @param[in] element The element to append as a child.
// @param[in] dom_element True if the element is to be part of the DOM, false otherwise.
void AppendChild(Element* element, bool dom_element = true);
```

If you parent the element using `InsertBefore()` instead of `AppendChild()`, the new element will be hidden if the element it was inserted adjacent to is hidden.

### Accessing hidden elements

Elements segregate their children by their hidden status. Visible children are always placed before, and therefore have a lower index, than hidden children. By default, the element's function `GetNumChildren()` will return the number of visible elements. To find the total number of elements including hidden elements, pass the boolean `true` into the function.

```cpp
// Get the current number of children in this element
// @param[in] include_non_dom_elements True if the caller wants to include the non-DOM children.
// @return The number of children.
int GetNumChildren(bool include_non_dom_elements = false) const;
```

The following code will iterate over all hidden children of an element:

```cpp
for (int index = element->GetNumChildren(); index < element->GetNumChildren(true); ++index)
	hidden_element = element->GetChild(index);
```

### Formatting hidden elements

Custom elements typically size and position their hidden elements internally when they receive a "resize" event.

#### Sizing

Hidden elements can be sized by calling the `SetBox()` function. `SetBox()` takes a `{{page.lib_ns}}::Core::Box` structure, which contains sizes for a two-dimensional content area and per-edge padding, borders and margin (see the RCSS documentation for more information on the [box model]({{"pages/rcss/box_model.html"|relative_url}})).

```cpp
// Sets the box describing the size of the element, and removes all others.
// @param[in] box The new dimensions box for the element.
void SetBox(const {{page.lib_ns}}::Core::Box& box);
```

You can either construct the box yourself, or use the static `BuildBox()` function on `{{page.lib_ns}}::Core::ElementUtilities`:

```cpp
// Generates the box for an element.
// @param[out] box The box to be built.
// @param[in] containing_block The dimensions of the content area of the block containing the element.
// @param[in] element The element to build the box for.
// @param[in] inline_element True if the element is placed in an inline context, false if not.
static void BuildBox(Box& box, const {{page.lib_ns}}::Core::Vector2f& containing_block, Element* element, bool inline_element = false);
```

`BuildBox()` will generate the values of a `{{page.lib_ns}}::Core::Box` from the `width`{:.prop}, `max-width`{:.prop}, `min-width`{:.prop}, and `height`{:.prop}, `max-height`{:.prop} and `min-height`{:.prop} properties set on an element. The parameters are:

* `box`: The box to be generated.
* `containing_block`: The element's containing block. This is typically the size of the content area of the containing element, but does not have to be.
* `element`: The element to generate the box for.
* `inline_element`: True if the element is inline, false if not. Generally you want to leave this as false. 

The following code will generate and set the box on a hidden element from within its parent:

```cpp
{{page.lib_ns}}::Core::Box box;
{{page.lib_ns}}::Core::ElementUtilities::BuildBox(box, GetBox().GetContentArea(), hidden_element);
hidden_element->SetBox(box);
```

But if you want to force the hidden element to be a certain size, instead you might do:

```cpp
{{page.lib_ns}}::Core::Box box;
box.SetContent({{page.lib_ns}}::Core::Vector2f(100, 150));
box.SetEdge({{page.lib_ns}}::Core::Box::BORDER, {{page.lib_ns}}::Core::Box::TOP, 1);
hidden_element->SetBox(box);
```

#### Positioning

To set the position of a hidden element, use the `SetOffset()` function. This sets the two-dimensional offset of the element top-left border edge from another element's top-left border edge. Typically, a custom element will position an internal hidden element relative to itself, but this is not required.

```cpp
// Sets the position of this element, as a two-dimensional offset from another element.
// @param[in] offset The offset (in pixels) of our primary box's top-left border corner from our offset parent's top-left border corner.
// @param[in] offset_parent The element this element is being positioned relative to.
// @param[in] offset_fixed True if the element is fixed in place (and will not scroll), false if not.
void SetOffset(const {{page.lib_ns}}::Core::Vector2f& offset,
               {{page.lib_ns}}::Core::Element* offset_parent,
               bool offset_fixed = false);
```

However, `{{page.lib_ns}}::Core::ElementUtilities` has a number of functions to aid in positioning a hidden element.

```cpp
// Sizes and positions an element within its parent.
// @param element[in] The element to size and position.
// @param offset[in] The offset of the element inside its parent's content area.
static bool PositionElement({{page.lib_ns}}::Core::Element* element,
                            const {{page.lib_ns}}::Core::Vector2f& offset);
```

`PositionElement()` resizes an element (using `BuildBox()`) and positions it within its parent. As positioning border-corner to border-corner can be quite confusing, this function treats the offset as between the content areas of the elements.

```cpp
// Sizes an element, and positions it within its parent offset from the borders of its content area.
// @param element[in] The element to size and position.
// @param offset[in] The offset from the parent's borders.
// @param anchor[in] Defines which corner or edge the border is to be positioned relative to.
static bool PositionElement({{page.lib_ns}}::Core::Element* element,
                            const {{page.lib_ns}}::Core::Vector2f& offset,
                            {{page.lib_ns}}::Core::ElementUtilities::PositionAnchor anchor);
```

There is also an override for `PositionElement()` for positioning an element offset from a specific corner or edge of its parent, not just the top-left corner. The third parameter, `anchor`, can be one or more of the `PositionAnchor` enumeration OR'ed together:

```cpp
enum PositionAnchor
{
	TOP = 1 << 0,
	BOTTOM = 1 << 1,
	LEFT = 1 << 2,
	RIGHT = 1 << 3,

	TOP_LEFT = TOP | LEFT,
	TOP_RIGHT = TOP | RIGHT,
	BOTTOM_LEFT = BOTTOM | LEFT,
	BOTTOM_RIGHT = BOTTOM | RIGHT
};
```

#### Invoking the layout engine

{{page.lib_name}}'s internal layout engine can be run on a hidden element to format the element's visible descendants. To do so, call the static `FormatElement()` function on `{{page.lib_ns}}::Core::ElementUtilities`.

```cpp
// Formats the contents of an element.
// @param[in] element The element to lay out.
// @param[in] containing_block The size of the element's containing block.
static bool FormatElement({{page.lib_ns}}::Core::Element* element,
                          const {{page.lib_ns}}::Core::Vector2f& containing_block);
```

### Formatting hidden text elements

It possible to append text elements as hidden elements. In this case, you will need to use the `{{page.lib_ns}}::Core::ElementText` API to get the element to generate and position strings of characters.

#### Generating lines of text

Once a text element has had raw text set on it (through the `SetText()` function), you can call `GenerateString()` to generate a character sequence for rendering on a single line. Depending on the length of the raw text and the available width, you may need to call `GenerateString()` multiple times to generate all the lines required to render the element's content.

```cpp
// Generates a line of text rendered from this element.
// @param[out] line The characters making up the line, with white-space characters collapsed and endlines processed appropriately.
// @param[out] line_length The number of characters from the source string consumed making up this string.
// @param[out] line_width The width (in pixels) of the generated line.
// @param[in] line_begin The index of the first character to be rendered in the line.
// @param[in] maximum_line_width The width (in pixels) of space allowed for the line, or -1 for unlimited space.
// @param[in] right_spacing_width The width (in pixels) of the spacing that must be remaining on the right of the line if this is the final line.
// @param[in] trim_whitespace_prefix If we're collapsing whitespace, whether or not to remove all prefixing whitespace or collapse it down to a single space.
// @return True if the line reached the end of the element's text, false if not.
bool GenerateLine({{page.lib_ns}}::Core::String& line,
                  int& line_length,
                  float& line_width,
                  int line_begin,
                  float maximum_line_width,
                  float right_spacing_width,
                  bool trim_whitespace_prefix);
```

The parameters to this function are:

* `line`: The string the contents of the generated line will be written to.
* `line_length`: An integer to store the number of characters used by the source string to generate this line. Because of whitespace processing, this value may be greater than the length of the generated line.
* `line_width`: A floating-point value to store the width of the generated string, in pixels.
* `line_begin`: The index of the first character in the source string to begin generating the line from.
* `maximum_line_width`: The maximum length (in pixels) the line can be.
* `right_spacing_width`: If the generated line is the last line required by the text node, then this space (in pixels) must be available to the right of the line. This is not generally required by custom text layouts.
* `trim_whitespace_prefix`: If this is set to true, collapsed whitespace will be trimmed from the front of the line. This is usually set to false for the first line, true for the second and subsequent line. 

The function will return true if the generated line is the last line required to render the content of the element, false if further lines are required.

The following code sample will generate all of the lines required for a text node, each line being allowed a maximum width of 200 pixels:

```cpp
{{page.lib_ns}}::Core::ElementText* text_element = document->CreateTextNode("sample text");

int line_begin = 0;
bool last_line = false;

while (!last_line)
{
	{{page.lib_ns}}::Core::String line;
	int line_length = 0;
	float line_width = 0;

	last_line = text_element->GenerateString(line, line_length, line_width, line_begin, 200, 0, line_begin > 0);
	line_begin += line_length;
}
```

The `GenerateString()` will format whitespace and endlines as appropriate for the value of the `white-space`{:.prop} RCSS property on the element. To change how it processes whitespace, change the `white-space`{:.prop} property.

Just generating lines of text from the element won't position them or get them rendering however.

### Rendering text

Text elements store a list of generated lines, each with a two-dimensional offset from the top-left of the text element. To begin positioning lines, call `ClearLines()` to clear all previously-generated lines.

```cpp
// Clears all lines of generated text and prepares the element for generating new lines.
void ClearLines();
```

Then call `AddLines()` for each generated line.

```cpp
// Adds a new line into the text element.
// @param[in] line_position The position of this line, as an offset from element.
// @param[in] line The contents of the line.
void AddLine(const {{page.lib_ns}}::Core::Vector2f& line_position,
             const {{page.lib_ns}}::Core::String& line) = 0;
```

The following code sample extends the previous sample by placing each line of text as it is generated:

```cpp
{{page.lib_ns}}::Core::ElementText* text_element = document->CreateTextNode("sample text");

int line_begin = 0;
bool last_line = false;
float position = 0;

while (!last_line)
{
	{{page.lib_ns}}::Core::String line;
	int line_length = 0;
	float line_width = 0;

	last_line = text_element->GenerateString(line, line_length, line_width, line_begin, 200, 0, line_begin > 0);
	line_begin += line_length;

	text_element->AddLine(line, {{page.lib_ns}}::Core::Vector2f(position, 0));
	position += (float) {{page.lib_ns}}::Core::ElementUtilities::GetLineHeight(text_element);
}
```

### Examples

You can see plenty of examples of using hidden elements in the Controls plugin, particularly in the select form control (`ElementFormControlSelect.cpp`{:.path} and `WidgetDropDown.cpp`{:.path}), or for text layout, the text area control (`ElementFormControlText.cpp`{:.path} and `WidgetTextInput.cpp`{:.path}). 