---
layout: page
title: Shadow Effect
parent: rcss/font_effects
---

The shadow font effect renders a coloured copy of text with an offset, giving the effect of a shadow.

![effect_shadow.jpg](effect_shadow_1.jpg)

The effect has the following properties:

*offset-x*

Value: | \<number\>
Initial: | 0
Percentages: | N/A

*offset-y*

Value: | \<number\>
Initial: | 0
Percentages: | N/A

These properties define the offset, in pixels, between the source text and the shadow.

*offset*

A shorthand property for setting offset-x and offset-y.

```
/* Declares a font effect shadow. */
h1
{
    header-font-effect: shadow;
    header-offset: 2px 2px;
    header-colour: black;
}
```
