---
layout: page
title: RML Data Display Elements
description: RML Data Display Elements
---

### DATAGRID Element

*Attributes*

**source** = datasource (CS)
>The name of the data source and table that the rows of the data grid will be fetched from.

#### COL Element

The COL represents a column in the datagrid. Each column displays information about each row in the data source in the form of one or more fields.

*Attributes*

**fields** = cdata (CS)
>A comma-separated list of fields that this column will fetch from the data source about each row and display.

**formatter** = cdata (CS)
>The name of the data formatter to use to turn the raw field information into RML. If not set (or the formatter can't be found) then the raw text will be displayed.

**width** = number (CN)
>The width (in px or %) of the column.

### DATASELECT Element

The DATASELECT Element is described in the [Forms](forms.html#dataselect-element) section.
