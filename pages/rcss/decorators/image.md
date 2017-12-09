---
layout: page
title: Image decorator
parent: rcss/decorators
---

The 'image' decorator can render a single image or a rectangular subsection of a single image. It has the following properties:

*image-src*

Value: | \<string\>
Initial: | not defined
Percentages: | N/A

This property defines the name (and, for file sources, the relative path) of the source image.

*image-s-begin, image-t-begin*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 0
Percentages: | N/A

*image-s-end, image-t-end*

Value: | \<number\> \| \<length\> \| \<percentage\>
Initial: | 1
Percentages: | N/A

These values specify the texture coordinates to use when rendering the image. Values have the following meanings:

number
    Expected to be a floating-point number from 0 to 1, this is the raw texture coordinate. 
length
    Expected to be expressed in 'px' units and from 0 to the length of the appropriate texture axis. The texture will be rendered from (for -begin) or to (for -end) this pixel. 
percentage
    The texture will be rendered from (for -begin) ot to (for -end) this far across the appropriate texture axis. 

*image-s*

A shorthand property for setting image-s-begin and image-s-end.

*image-t*

A shorthand property for setting image-t-begin and image-t-end.

*image*

A shorthand property for setting image-src, image-s-begin, image-t-begin, image-s-end and image-t-end.

```css
/* Declares an image decorator. */
div#avatar
{
    avatar-decorator: image;
    avatar-image: player.png 0px 32px 32px 64px;
}
```

The image will be stretched or shrunk to render across the entire padded area of an element it is attached to.
