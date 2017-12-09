---
layout: page
title: Element Index
parent: rml
---

The following is a list of elements supported by RML.

Tag                                    | Attributes
-------------------------------------- | ---
[img](images.html#img-element)         | src="CDATA" width="NUMBER" height="NUMBER"
[handle](controls.html#handle-element) | movetarget="IDREF" sizetarget="IDREF"

The following is a list of elements supported when the Controls plugin is used.

Tag                                            | Attributes
---------------------------------------------- | ---
[col](data_display.html#col-element)           | fields="CDATA" formatter="CDATA" width="NUMBER"
[datagrid](data_display.html#datagrid-element) | source="DATASOURCE"
[dataselect](forms.html#dataselect-element)    | source="DATASOURCE" fields="CDATA" formatter="CDATA" valuefield="CDATA"
[form](forms.html#form-element)                | 
[input](forms.html#input-element)              | type="text\|radio\|checkbox\|submit\|range" value="CDATA" size="NUMBER" maxlength="NUMBER" min="NUMBER" max="NUMBER" step="NUMBER" orientation="CDATA"
[option](forms.html#option-element)            | selected="CDATA" unselectable="CDATA"
[select](forms.html#select-element)            | value="CDATA"
[tabset](controls.html#tabset-element)         | 
[textarea](forms.html#textarea-element)        | cols="NUMBER" rows="NUMBER" maxlength="NUMBER" wrap="CDATA" 