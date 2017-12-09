---
layout: page
title: Decorators
parent: rcss
---

Decorators are an extension to CSS for RCSS. A decorator can be declared and named in a style sheet like a property, and then configured with decorator-specific properties. Custom decorator types can be developed to suit the needs of the user, and in this manner any kind of decoration can be applied to an element.

**Important**: Note that decorators can only be declared and defined in style sheets, not in style defined inline to an element.

### Declaring decorators

The declaration of a decorator is a property of the form:

```
<name>-decorator: <type>;
```

where <name> is the user-specified name of the decorator to declare, and <type> is the application-specific type of decorator. Rocket ships with the decorators 'image', 'tiled-box', 'tiled-horizontal' and 'tiled-vertical'. The decorator type 'none' is a reserved name that represents no decorator. For example, in the following:

```
button
{
    icon-decorator: tiled-horizontal;
}
```

the RCSS is attaching a decorator of type 'tiled-horizontal' called 'icon' to all 'button' elements. Note that the name 'icon' could be anything; it is only used to uniquely identify the decorator for applying properties to it.

Decorator types can be overridden like any other property. So, in the example:

```
h1
{
    icon-decorator: image;
}

h1:hover
{
    icon-decorator: none;
}
```

all 'h1' tags will have an image decorator attached, except when they are being hovered, when the decorator will not be rendered.

### Decorator properties

Decorators have their own property specifications with properties and shorthands, and these can be defined for declared decorators just like any other property. Decorator properties are set in the form:

```
<decorator_name>-<property_name>: <property_value>;
```

where <decorator_name> is the name the decorator was declared under, <property_name> is the name of the decorator's property to set, and <property_value> is the value of the property. For example:

```
h1
{
    icon-decorator: image;
    icon-image-src: ../images/header_arrow.png;
    icon-image-s-begin: 5px;
}
```

an 'image' decorator is declared, called 'icon'. On this decorator, the properties 'image-src' is set to '../images/header_arrow.png' and 'image-s-begin' is set to '5px'. The other properties in the 'image' specification are left to their defaults.

The decorator properties behave like other properties, so are subject to precedence and the cascade. For example, if the following RCSS was loaded after the previous example:

```
h1
{
    icon-image-s-begin: 3px;
}

h1:first-child
{
    icon-decorator: none;
}
```

all 'h1' tags will have their 'icon' decorators defined with an 's-begin' property of '3px' instead of '5px', and an 'h1' that is a first child will have no decorator.

Be careful of name conflicts! For example, in the following sample:

```
h1
{
    icon-decorator: image;
}

.secondary
{
    icon-decorator: tiled-horizontal;
}
```

an element of type 'h1' with a class of 'secondary' will have a single decorator, 'icon', of type 'tiled-horizontal'. If the decorators had different names then they both would be present.

### Specifying render order

If an element has more than one decorator on it, you can use the 'z-index' decorator property to control the render order in a similar fashion to elements. For example, in this sample:

```
button
{
    background-decorator: tiled-horizontal;
    background-z-index: 0;

    icon-decorator: image;
    icon-z-index: 1;
}
```

the 'icon' decorator will appear on top of the 'background' decorator. If no z-index is specified on a decorator, it will have the default z-index of 0.

### Decorators and pseudo-classes

Because of the way Rocket compiles pseudo-class rules within style sheets, overriding decorator properties within dynamic pseudo-classes (:active, :hover and :focus, not any of the structural classes) might not always work the way you think it should.

In a nutshell, each decorator will only process properties from their element's base rules (ie, those not specifying dynamic pseudo-classes in their final simple selector) and one rule specifying a dynamic pseudo-class (or classes), that with the highest specificity. For example, in the following sample:

```
button
{
    background-decorator: tiled-horizontal;
    background-image-src: ../button.png;
    background-image-s-end: 128px;
}

button:hover
{
    background-image-t-begin: 32px;
    background-image-t-end: 64px;
}

button:active
{
    background-image-s-begin: 128px;
    background-image-s-end: 256px;
}
```

if both the 'hover' and 'active' pseudo-classes are active on a 'button' element, the image-s-begin / end properties from the 'active' rule will be set, but the image-t-begin / -end properties from the 'hover' rule will be ignored. The 'active' rule is used as it has higher precedence that the 'hover' rule, appearing further down the file. If you wanted both the s and t properties set, you could create another rule:

```
button:active:hover
{
    background-image-t-begin: 32px;
    background-image-t-end: 64px;
    background-image-s-begin: 128px;
    background-image-s-end: 256px;
}
```

### Rocket decorators

Rocket comes with several built-in decorators for displaying images and tiled images behind elements.

1. For displaying a single stretched image, the [image](decorators/image.html) decorator.
2. For tiling images horizontally, the [tiled-horizontal](decorators/tiled_horizontal.html) decorator.
3. For tiling images vertically, the [tiled-vertical](decorators/tiled_vertical.html) decorator.
4. For tiling images across a box, the [tiled-box](decorators/tiled_box.html) decorator. 