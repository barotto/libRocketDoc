---
layout: page
title: Font Effects
parent: rcss
---

Font effects are an extension to CSS for RCSS for applying effects, such as outlining or shadowing, to text. Similarly to [decorators](decorators.html), font effects are declared and named in a style sheet like a property, and configured with font-effect-specific properties. Custom font effects can be developed to apply arbitrary effects onto text.

### Properties

Font effects are declared and configured within style sheets in exactly the same manner as decorators (except they are declared as 'font-effect' rather that 'decorator'), including the 'z-index' property to resolve an ambiguous render order. The only exceptions are noted below:

#### Colour

All font effects have a 'color' property which is applied multiplicatively over the entire effect.

#### Inheritance

Unlike decorators, font effects are inherited from parent elements. For example, the following declaration:

```css
h1
{
    header-font-effect: outline;
    header-width: 2px;
    header-color: black;
}
```

will add an outline effect on the text within all 'h1' elements and their descendants. To prevent inheritance, override the effect with 'none'. For example, to prevent the 'h1' outline effect from affecting 'span' elements, you could specify the following:

```css
h1 span
{
    header-font-effect: none;
}
```

### Rocket effects

Rocket comes with two built-in font effects:

1. For rendering shadows, the [shadow](font_effects/shadow.html) effect.
2. For outlining text, the [outline](font_effects/outline.html) effect. 