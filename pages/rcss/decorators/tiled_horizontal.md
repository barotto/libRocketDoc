---
layout: page
title: Tiled Horizontal decorator
parent: rcss/decorators
---

The 'horizontal-tiled' decorator can render three images, or subsections of images, horizontally across an element. One image is placed on the left edge, another on the right edge, and the last is stretched or repeated across the middle.

*left-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*left-image-s-begin, left-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*left-image-s-end, left-image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the left image. They behave similarly to the properties in the 'image' decorator.

*right-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*right-image-s-begin, right-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*right-image-s-end, right-image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the right image. They behave similarly to the properties in the 'image' decorator. If only one side is defined, the other will be the horizontal mirror of it. At least one side must be defined.

*center-image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

*center-image-repeat*

Value: | stretch \| clamp-stretch \| clamp-truncate \| repeat-stretch \| repeat-truncate
Initial: | stretch
Percentages: | N/A

Values have the following meanings:

stretch
>The image will be stretched to fit the centre of the decorator. 

clamp-stretch
>The image will be clamped to the upper-left of the centre of the decorator. If the centre of the decorator is too small to fit the image, it will be scaled down to fit. 

clamp-truncate
>The image will be clamped to the upper-left of the centre of the decorator. If the centre of the decorator is too small to fit the image, it will be truncated. 

repeat-stretch
>The image will be repeated across the centre of the decorator. Any last repeat will be stretched across the remainder. 

repeat-truncate
>The image will be repeated across the centre of the decorator. The last repeat will be truncated. 

*center-image-s-begin, center-image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*center-image-s-end, center-image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These properties specify the centre image. They behave similarly to the properties in the 'image' decorator.

*left-image-s*

Shorthand for setting left-image-s-begin and left-image-s-end.

*left-image-t*

Shorthand for setting left-image-t-begin and left-image-t-end.

*left-image*

A shorthand property for setting left-image-src, left-image-s-begin, left-image-t-begin, left-image-s-end and left-image-t-end.

*right-image-s*

Shorthand for setting right-image-s-begin and right-image-s-end.

*right-image-t*

Shorthand for setting right-image-t-begin and right-image-t-end.

*right-image*

A shorthand property for setting right-image-src, right-image-s-begin, right-image-t-begin, right-image-s-end and right-image-t-end.

*center-image-s*

Shorthand for setting center-image-s-begin and center-image-s-end.

*center-image-t*

Shorthand for setting center-image-t-begin and center-image-t-end.

*center-image*

A shorthand property for setting center-image-src, center-image-repeat, center-image-s-begin, center-image-t-begin, center-image-s-end and center-image-t-end.

The decorator renders across the padded area of its element. 