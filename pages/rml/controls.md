---
layout: page
title: RML Controls
description: RML Controls
---

### HANDLE Element

*Attributes*

**movetarget** = idref (CI)
>If specified, the handle will move the element specified by the ID when dragged. Can be "#document" to reference the parent document.

**sizetarget** = idref (CI)
>If specified, the handle will size the element specified by the ID when dragged. Can be "#document" to reference the parent document.

### TABSET Element

A TABSET element contains TAB elements and PANEL elements.

#### TAB Element

Each TAB element acts as a button, that when clicked will hide the currently visible panel and show its corresponding panel.

#### PANEL Element

A PANEL element is the body of the tabset. The visibility of the panel is controlled by the tab elements in the parent tabset. 