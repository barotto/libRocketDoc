---
layout: page
title: Visual effects
parent: rcss
---

### Overflow and clipping

The contents of a block box may extend beyond the content area of the box itself, for example under the following scenarios:

* The contents of an inline box wider than its containing block box cannot be broken.
* A block box has a width greater than its containing block box.
* A block box has an explicit height set, and its contents exceed that height. 

When overflow occurs, the 'overflow-x' and 'overflow-y' properties dictate how the overflow is handled.

#### Overflow: the 'overflow' property

*overflow-x, overflow-y*

Value: | visible \| hidden \| auto \| scroll
Initial: | visible
Applies to: | block-level elements
Inherited: | no
Percentages: | N/A

The values have the following meanings:

visible
>Overflowing content is visible along this axis.

hidden
>Overflowing content is hidden along this axis.

auto
>If overflow occurs along this axis, overflowing content is hidden and a scrollbar is generated and positioned along the axis so the hidden content can be scrolled into view. 

scroll
>A scrollbar is always visible along the axis, allowing hidden content to be scrolled into view. This will eliminate 'popping' if content suddenly overflows and a scrollbar appears. 

Note that, unlike CSS, if either of 'overflow-x' or 'overflow-y' is set to a value other that 'visible', clipping will occur on both axes.

*overflow*

Shorthand for overflow-x overflow-y. If two values are specified, the first will be used to specify overflow-x and the second overflow-y. If one value is specified, if will be used to specify both.

```css
/* Hide horizontal overflowing content and generate a scrollbar (if required) along the vertical axis. */
div#content
{
    overflow: hidden auto;
}
```

#### Clipping: the 'clip' property

This property is completely different from the CSS 'clip' property. Instead of defining the clipping region of an element, this property defines how the element interacts with the clipping regions of its ancestors.

In RCSS, the clipping region is always the 'client area', which is either the content or padded area (depending on the element).

*clip*

Value: | auto \| none \| \<number\>
Initial: | auto
Applies to: | all elements
Inherited: | no
Percentages: | N/A

The values have the following meanings:

auto
>The element is subjected to all the clipping regions put in place by its ancestors. 

none
>The element is never clipped (except by the context). 

number
>The element is subjected to the clipping regions of its ancestors, except it skips the closest \<number\> ancestors that could have put in place a clipping region (ie, those ancestors with an 'overflow-x' or 'overflow-y' other than 'visible'). 

### Visibility: the 'visibility' property

*visibility*

Value: | visible \| hidden
Initial: | visible
Applies to: | all elements
Inherited: | no
Percentages: | N/A

Values have the following meanings:

visible
>The generated box is visible. 

none
>The generated box, and all of its descendants, is visible. Note that the box still has an impact on layout, it is just not rendered. 