# Fitz

If you wonder where the prefix “fz” and name Fitz come from, MuPDF originally
started out as a prototype of a new rendering library architecture for
Ghostscript. It was to be a fusion of libart and Ghostscript. History turned
out differently, and the project mutated into a standalone PDF renderer now
called MuPDF. The “fz” prefix for the graphics library and core modules remains
to this day.

* [Context](context.md)
* [Errors](error.md)
* [Memory](memory.md)
  * [Malloc & Free](memory.md#malloc-free)
  * [Pool Allocator](memory.md#pool-allocator)
  * [Reference Counting](memory.md#reference-counting)
* [Colors](colors.md)
  * [Color Management](colors.md#color-management)
  * [Device colorspaces](colors.md#device-colorspaces)
  * [Indexed colorspaces](colors.md#indexed-colorspaces)
  * [ICC colorspaces](colors.md#icc-colorspaces)
  * [Color converters](colors.md#color-converters)
* [Data Structures](data-structures.md)
  * [Hash Table](data-structures.md#hash-table)
  * [Binary Tree](data-structures.md#binary-tree)
* [Buffers & Streams](io.md)
  * [Buffers](io.md#buffers)
  * [Input Streams](io.md#input-streams)
  * [Filters](io.md#filters)
  * [Output Streams](io.md#output-streams)
  * [File Archives](io.md#file-archives)
* [Math](math.md)
  * [fz_point](math.md#fz-point)
  * [fz_rect and fz_irect](math.md#fz-rect-and-fz-irect)
  * [fz_matrix](math.md#fz-matrix)
  * [fz_quad](math.md#fz-quad)
  * [List of math functions](math.md#list-of-math-functions)
* [Pixmaps](pixmap.md)
* [Strings](strings.md)
  * [Unicode](strings.md#unicode)
  * [Locale Independent](strings.md#locale-independent)
  * [Formatting](strings.md#formatting)
* [XML Parser](xml.md)

<!-- TODO
archive.h
band-writer.h
barcode.h
bidi.h
bitmap.h
buffer.h
color.h
compress.h
compressed-buffer.h
config.h
context.h
crypt.h
deskew.h
device.h
display-list.h
document.h
export.h
filter.h
font.h
geometry.h
getopt.h
glyph-cache.h
glyph.h
hash.h
heap-imp.h
heap.h
image.h
json.h
link.h
log.h
outline.h
output-svg.h
output.h
path.h
pixmap.h
pool.h
separation.h
shade.h
store.h
story-writer.h
story.h
stream.h
string-util.h
structured-text.h
system.h
text.h
track-usage.h
transition.h
tree.h
types.h
util.h
version.h
write-pixmap.h
writer.h
xml.h -->
