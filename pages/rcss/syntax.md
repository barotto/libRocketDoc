---
layout: page
title: Syntax
parent: rcss
---

A style sheet is made up of a number of rules. Each rule has a number of selectors, which define which elements the rule applies to, and properties, which are applied to those elements.

The basic syntax of a rule is as follows:

```css
selector1,
selector2,
selector3
{
	property1: value1;
	property2: value2;
	property3: value3;
}
```

Selectors are comma-separated, followed by the rule block which is defined by curly braces. Inside the rule block is a list of semi-colon separated property declarations; each property declaration is made up of the property to set, a colon, and a space-separated list of the values to assign to that property. The values accepted by each value, and how many they require, is specific to each value.

### Comments

Comments can be included in a style sheet using the C-style /* and */ characters.

### Values

Which values each property accepts is given in their definition. Values may be keywords, which are set as specific strings such as auto, left, etc, or generic strings (such as font names or file paths), or one of the types described below.

#### Numbers

Specified as <number> in a property's Values list. A number can be an integer or real number.

```css
z-index: 16;
```

#### Lengths

Specified as <length> in a property's Values list. A length is a horizontal or vertical measurement consisting of a number and a unit. RCSS recognises the following units:

* `px`{:.value}: One px is equivalent to one pixel on the output medium.
* `em`{:.value}: One em is equivalent to the line-height of the current font.
* `ex`{:.value}: One ex is equivalent to the height of the current font's lower-case x.

Note that as RCSS is designed solely for outputting to a computer monitor, it does not recognize the physical measurements **in**, **cm**, **mm**, **pt** or **pc**.

```css
width: 125px;
```

#### Percentages

Specified as `<percentage>`{:.value} in the property's Values list. A percentage value is evaluated relative to some other value, which is specified in each property that supports a percentage. For example, width can be expressed as a percentage, which is evaluated against the width of the element's containing block.

```css
min-height: 50%;
```

#### Colours

Specified as `<colour>`{:.value} in the property's Values list. Colours represent a RGBA value, and can be declared in numerous ways:

* As the name of one of the 16 colours defined in the HTML 4.0 specification (aqua, black, blue, fuchsia, gray, green, lime, maroon, navy, olive, purple, red, silver, teal, white, and yellow), plus grey (alias for gray), orange and transparent.
* Prefixed with the '#' character, followed by 3, 4, 6 or 8 hexadecimal digits. 3 or 6 digits represent an RGB triplet, and will have 255 attached as the opacity. If only 3 are specified, each digit will be replicated before being read; for example, #FE0 is equivalent to #FFEE00. 4 or 8 digits allow the specification of a translucent colour.
* In the format `rgb(r, g, b)`{:.value} or `rgba(r, g, b, a)`{:.value}, where each of red, green, blue (and optionally alpha) is specified as a value from 0 to 255. An rgb value has an alpha of 255 attached.
* In the format `rgb(r%, g%, b%)`{:.value} or `rgba(r%, g%, b%, a%)`{:.value}, where each of red, green, blue (and optionally alpha) is specified as a percentage value from 0 to 100. An rgb value will have full opacity. 

**Important**: Note that the declaration of the alpha channel when using the rgba keyword differs from the HTML5 specification.

So, for example, the following colour declarations are identical:

```css
color: red;
color: #F00;
color: #FF0000FF;
color: rgb(100%, 0%, 0%);
color: rgba(100%, 0%, 0%, 100%);
color: rgba(255, 0, 0, 255);
```

### Referencing RCSS from RML

A style sheet can be either stored in an external file (usually with the extension .rcss) and referenced from an RML file, or declared inline inside an RML file. Referencing an external RCSS file is done using the `<link>`{:.tag} tag in the following manner:

```html
<html>
	<head>
		<link type="text/css" href="sample.rcss" />

	...
```

File paths are relative to the referencing document.

Declaring an inline style sheet is done using the `<style>`{:.tag} tag, also within the <head> tag:

```html
<html>
	<head>
		<style>
			body
			{
				margin: 0px;
			}
		</style>

	...
```

Multiple style sheets can be included in a single document and combined with inline style declarations. The ordering of style declarations is important, as they may be used to resolve the precedence conflicting style sheet rules.

Also, style sheet properties can be declared directly on an element. This is done by inserting semi-colon separated style sheet property declarations into the `style`{:.attr} attribute of an element. For example, the following RML fragment:

```html
<div style="width: 25%; min-width: 55px;">
</div>
```

sets the `width`{:.prop} property to '25%' and the `min-width`{:.prop} property to '55px' on the `div`{:.tag} element. 