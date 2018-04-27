---
layout: page
title: RML Document Structure
parent: rml
---

### RML Element

All Rocket documents begin with the RML element. The element should contain two children, HEAD and BODY.

### HEAD Element

The HEAD element contains information about the current document, such as its title, style and template information is references. No information in the header is rendered.

### TITLE Element

The TITLE element contains the title of the document. This is often used for the specifying the contents of the title bar of a game window.

### LINK Element

The LINK element is used to specify additional resources the document requires.

*Attributes*

**type** = cdata (CI)

>Type of link, which should be one of:
>* text/rcss - [Rocket Style Sheet Specification](../rcss.md)
>* text/template - [Rocket Template](templates.md)
>* text/script - Rocket script

**href** = cdata (CS)

>Specifies the source URI, relative to the document being parsed.

### BODY Element

The body of a document contains the document's content. All elements within the body tag is laid out and rendered by Rocket based on the active style sheets. 