---
layout: page
title: Property index
parent: rcss
---

Following is a full index of properties recognised by RCSS. The **Notes** column details important changes from the CSS specification.

Name | Values | Initial value | Applies to | Inherited? | Percentages | Notes
---- | ------ | ------------- | ---------- | ---------- | ----------- | -----
'background' | 'background-color' | | | | | Excludes images.
'background-color' | \<colour\> | transparent | all | no | | 
'border-color' | 'border-top-color' 'border-right-color' 'border-bottom-color' 'border-left-color' | | | | | 
'border-top' 'border-right' 'border-bottom' 'border-left' | 'border-\<edge\>-width' 'border-\<edge\>-color' | | | | | Excludes border style.
'border-top-color' 'border-right-color' 'border-bottom-color' 'border-left-color' | \<color\> | black | all | no | N/A | 
'border-top-width' 'border-right-width' 'border-bottom-width' 'border-left-width' | \<length\> \| \<percentage\> | 0px | all | no | width of containing block | 
'border-width' | 'border-top-width' 'border-right-width' 'border-bottom-width' 'border-left-width' | | all | | | 
'bottom' | \<length\> \| \<percentage\> | 0px | positioned elements | no | height of containing block | No 'auto'.
'clear' | left \| right \| both \| none | none | block-level elements | no | N/A | 
'clip' | \<number\> \| auto \| none | auto | all | yes | N/A | Controls interaction with ancestor element's clipping regions.
'color' | \<colour\> | black | all | yes | N/A | 
'cursor' | \<string\> \| auto | auto | all | yes | N/A | \<string\> refers to title of cursor document.
'display' | inline \| block \| inline-block \| none | inline | all | no | N/A | 
'drag' | none \| drag \| drag-drop \| block | none | all | no | N/A | Introduced for RCSS. Controls generation of drag messages.
'font' | 'font-style' 'font-weight' 'font-size' 'font-family' 'font-charset' | | | | | 
'font-charset' | \<urange\> | U+0020-007E | all | yes | N/A | Introduced for RCSS. Specifies required range of characters.
'font-family' | \<string\> | | all | yes | N/A | Only single family supported.
'font-size' | \<number\> \| \<percentage\> | 12 | all | yes | size of parent font | 
'font-style' | normal \| italic | normal | all | yes | N/A | 'oblique' not supported.
'font-weight' | normal \| bold | normal | all | yes | N/A | Intermediate weights not supported.
'height' | \<length\> \| \<percentage\> \| auto | auto | block and replaced inline elements | no | height of containing block | 
'left' | \<length\> \| \<percentage\> | 0px | positioned elements | no | width of containing block | No 'auto'.
'line-height' | \<number\> \| \<length\> \| \<percentage\> | 1.2 | all | yes | font size | 'normal' not supported.
'margin' | 'margin-top' 'margin-right' 'margin-bottom' 'margin-left' | | | | | 
'margin-top' 'margin-right' 'margin-bottom' 'margin-left' | \<length\> \| \<percentage\> \| auto | 0px | all | no | width of containing block | 
'max-height' | \<length\> \| \<percentage\> | -1 | block and replaced inline elements | no | height of containing block | 'none' not supported, use negative numbers instead.
'min-height' | \<length\> \| \<percentage\> | 0px | block and replaced inline elements | no | height of containing block | 
'max-width' | \<length\> \| \<percentage\> | -1 | block and replaced inline elements | no | width of containing block | 'none' not supported, use negative numbers instead.
'min-width' | \<length\> \| \<percentage\> | 0px | block and replaced inline elements | no | width of containing block | 
'overflow' | 'overflow-x' 'overflow-y' | | | | | 
'overflow-x' | visible \| hidden \| scroll \| auto | visible | block elements | no | N/A | Content clipped if either axis is not 'visible'.
'overflow-y' | visible \| hidden \| scroll \| auto | visible | block elements | no | N/A | Content clipped if either axis is not 'visible'.
'padding' | 'padding-top' 'padding-right' 'padding-bottom' 'padding-left' | | | | | 
'padding-top' 'padding-right' 'padding-bottom' 'padding-left' | \<length\> \| \<percentage\> | 0px | all | no | width of containing block | 
'position' | static \| relative \| absolute \| fixed | static | all | no | N/A | 'fixed' is positioned like 'absolute' but ignores scrolling.
'right' | \<length\> \| \<percentage\> | 0px | positioned elements | no | width of containing block | No 'auto'.
'scrollbar-margin' | \<length\> | 0px | scrollbar-horizontal and scrollbar-vertical elements | no | N/A | Introduced for RCSS. Specifies a bottom / right margin (depending on orientation) that will collapse with the scrollbar on the complementary axis.
'tab-index' | none \| auto | none | all | yes | N/A | Introduced for RCSS. Controls order of focus switching when the tab key is pressed.
'text-align' | left \| right \| center | left | block-level elements | yes | N/A | 'justify' not supported.
'text-decoration' | underline \| none | none | all | yes | N/A | 'overline', 'line-through' not supported.
'top' | \<length\> \| \<percentage\> | 0px | positioned elements | no | height of containing block | No 'auto'.
'vertical-align' | baseline \| sub \| super \| text-top \| text-bottom \| middle \| top \| bottom \| \<percentage\> \| \<length\> | baseline | inline-level elements | no | line-height | 
'visibility' | visible \| hidden | visible | all | no | N/A | 
'white-space' | normal \| pre \| nowrap \| pre-wrap \| pre-line | normal | block-level elements | yes | N/A | 'pre-wrap' and 'pre-line' from CSS3.
'width' | \<length\> \| \<percentage\> \| auto | auto | block and replaced inline elements | no | width of containing block | 
'z-index' | \<number\> \| auto \| top \| bottom | auto | all | no | N/A | Applies to all elements. 'top' and 'bottom' introduced. For documents, 'auto' allows pulling to front, otherwise remains at top or bottom. 