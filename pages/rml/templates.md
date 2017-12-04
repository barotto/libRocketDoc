---
layout: page
title: RML Templates
description: RML Templates
---

### TEMPLATE Element

The TEMPLATE element has two uses, to define a template and inject a template inline into an existing RML documents.

When defining a template, TEMPLATE element should be used in place of RML.

*Attributes*

**name** = cdata (CI)
>The name of the template. Must be unique. Is used by other RML documents to reference the template.

**content** = idref (CI)
>The id of the element that the content will be put into.

When injecting a template, all elements inside the TEMPLATE tag will be placed inside the templates content element.

*Attributes*

**src** = cdata (CS)
>The name of the template to inject

### BODY Element

The BODY element has a template attribute that is a shorthand for injecting a template around the body tag.

*Attributes*

**template** = cdata (CS)
>The name of the template to use. All child elements under the BODY element will be loaded into the template.

