---
layout: page
title: Tiled Vertical decorator
parent: rcss/decorators
---

The 'tiled-vertical' decorator can render three images, or subsections of images, vertically across an element. One image is placed on the top edge, another on the bottom edge, and the last is stretched or repeated across the middle.

*top-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*top-image-s-begin, top-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*top-image-s-end, top-image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the top image. They behave similarly to the properties in the 'image' decorator.

*bottom-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*bottom-image-s-begin, bottom-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*bottom-image-s-end, bottom-image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the bottom image. They behave similarly to the properties in the 'image' decorator. If only one side is defined, the other will be the vertical mirror of it. At least one side must be defined.

*center-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*center-image-repeat*

Value: | stretch \| clamp-stretch \| clamp-truncate \| repeat-stretch \| repeat-truncate
Initial: | stretch
Percentages: | N/A

*center-image-s-begin, center-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*center-image-s-end, center-image-t-end*
Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the centre image. They behave similarly to the centre properties in the 'horizontal-tiled' decorator.

*top-image-s*

Shorthand for setting top-image-s-begin and top-image-s-end.

*top-image-t*

Shorthand for setting top-image-t-begin and top-image-t-end.

*top-image*

A shorthand property for setting top-image-src, top-image-s-begin, top-image-t-begin, top-image-s-end and top-image-t-end.

*bottom-image-s*

Shorthand for setting bottom-image-s-begin and bottom-image-s-end.

*bottom-image-t*

Shorthand for setting bottom-image-t-begin and bottom-image-t-end.

*bottom-image*

A shorthand property for setting bottom-image-src, bottom-image-s-begin, bottom-image-t-begin, bottom-image-s-end and bottom-image-t-end.

*center-image-s*

Shorthand for setting center-image-s-begin and center-image-s-end.

*center-image-t*

Shorthand for setting center-image-t-begin and center-image-t-end.

*center-image*

A shorthand property for setting center-image-src, center-image-repeat, center-image-s-begin, center-image-t-begin, center-image-s-end and center-image-t-end.

The decorator renders across the padded area of its element.
