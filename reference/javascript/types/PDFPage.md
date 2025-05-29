# PDFPage

A PDFPage inherits [Page](Page.md), so its interface is also available for
PDFPage objects.

## Constructors

### *class* PDFPage()

*You cannot create instances of this class with the new operator!*

Instances of this class are returned by [`loadPage`](Document.md#Document.prototype.loadPage) called on a [PDFDocument](PDFDocument.md).

## Static properties

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

### PDFPage.REDACT_IMAGE_NONE

for no image redactions

### PDFPage.REDACT_IMAGE_REMOVE

to redact entire images

### PDFPage.REDACT_IMAGE_PIXELS

for redacting just the covered pixels

### PDFPage.REDACT_IMAGE_UNLESS_INVISIBLE

only redact visible images

### PDFPage.REDACT_LINE_ART_NONE

for no line art redactions

### PDFPage.REDACT_LINE_ART_REMOVE_IF_COVERED

redacts line art if covered

### PDFPage.REDACT_LINE_ART_REMOVE_IF_TOUCHED

redacts line art if touched

### PDFPage.REDACT_TEXT_REMOVE

to redact text

### PDFPage.REDACT_TEXT_NONE

for no text redaction

## Instance methods

### PDFPage.prototype.getObject()

Get the underlying [PDFObject](PDFObject.md) for a [PDFPage]().

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = page.getObject()
```

### PDFPage.prototype.getAnnotations()

Returns an array of all annotations on the page.

* **Returns:**
  Array of [PDFAnnotation](PDFAnnotation.md)

```javascript
var annots = page.getAnnotations()
```

### PDFPage.prototype.getWidgets()

Returns an array of all widgets on the page.

* **Returns:**
  Array of [PDFWidget](PDFWidget.md)

```javascript
var widgets = page.getWidgets()
```

### PDFPage.prototype.setPageBox(box, rect)

Sets the type of box required for the page.

* **Arguments:**
  * **box** (`string`) – The [page box](../../common/glossary.md#term-Page-Box) type to change.
  * **rect** ([`Rect`](Rect.md#Rect)) – The new desired dimensions.

```javascript
page.setPageBox("ArtBox", [10, 10, 585, 832])
```

### PDFPage.prototype.createAnnotation(type)

Create a new blank annotation of a given annotation type.

* **Arguments:**
  * **type** (`string`) – The [annotation type](../../common/glossary.md#term-Annotation-Type) to create.
* **Returns:**
  [PDFAnnotation](PDFAnnotation.md).

```javascript
var annot = page.createAnnotation("Text")
```

### PDFPage.prototype.deleteAnnotation(annot)

Delete a [PDFAnnotation](PDFAnnotation.md) from the page.

* **Arguments:**
  * **annot** ([`PDFAnnotation`](PDFAnnotation.md#PDFAnnotation))

```javascript
var annots = getAnnotations()
page.delete(annots[0])
```

### PDFPage.prototype.update()

Loop through all annotations of the page and update them.

Returns true if re-rendering is needed because at least one
annotation was changed (due to either events or Javascript
actions or annotation editing).

* **Returns:**
  boolean

```javascript
// edit an annotation
var updated = page.update()
```

### PDFPage.prototype.toPixmap(matrix, colorspace, alpha, showExtras, usage, box)

Render the page into a [Pixmap](Pixmap.md) with the PDF “usage” and [page box](../../common/glossary.md#term-Page-Box).

* **Arguments:**
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The desired colorspace.
  * **alpha** (`boolean`) – Whether or not the returned pixmap will use alpha.
  * **renderExtra** (`boolean`) – Whether annotations and widgets should be rendered. Defaults to true.
  * **usage** (`string`) – If the output is destined for viewing or printing. Defaults to “View”.
  * **box** (`string`) – Default is “CropBox”.
* **Returns:**
  [Pixmap](Pixmap.md)

```javascript
var pixmap = page.toPixmap(
        mupdf.Matrix.identity,
        mupdf.ColorSpace.DeviceRGB,
        true,
        false,
        "View",
        "CropBox"
)
```

### PDFPage.prototype.applyRedactions(blackBoxes, imageMethod, lineArtMethod, textMethod)

Applies all the Redaction annotations on the page.

Redactions are a special type of annotation used to permanently remove (or
“redact”) content from a PDF.

* **Arguments:**
  * **blackBoxes** (`boolean`) – Whether to use black boxes at each redaction or not.
  * **imageMethod** (`number`) – Default is [`PDFPage.REDACT_IMAGE_PIXELS`](#PDFPage.REDACT_IMAGE_PIXELS).
  * **lineArtMethod** (`number`) – Default is [`PDFPage.REDACT_LINE_ART_REMOVE_IF_COVERED`](#PDFPage.REDACT_LINE_ART_REMOVE_IF_COVERED).
  * **textMethod** (`number`) – Default is [`PDFPage.REDACT_TEXT_REMOVE`](#PDFPage.REDACT_TEXT_REMOVE).

Once redactions are added to a page you can *apply* them, which is an
irreversible action, thus it is a two step process as follows:

```javascript
var annot1 = page.createAnnotation("Redaction")
annot1.setRect([0, 0, 500, 100])
var annot2 = page.createAnnotation("Redaction")
annot2.setRect([0, 600, 500, 700])
page.applyRedactions(true, mupdf.PDFPage.REDACT_IMAGE_NONE)
```

### PDFPage.prototype.process(processor)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Run through the page contents stream and call methods on the
supplied [PDFProcessor](PDFProcessor.md).

* **Arguments:**
  * **processor** ([`PDFProcessor`](PDFProcessor.md#PDFProcessor)) – User defined function callback object.

```javascript
pdfPage.process(processor)
```

### PDFPage.prototype.getTransform()

Return the transform from MuPDF page space (upper left page origin,
y descending, 72 dpi) to PDF user space (arbitrary page origin, y
ascending, UserUnit dpi).

* **Returns:**
  [Rect](Rect.md)

```javascript
var transform = pdfPage.getTransform()
```

### PDFPage.prototype.createSignature(name)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Create a new signature widget with the given name as field
label.

* **Arguments:**
  * **name** (`string`) – The desired field label.
* **Returns:**
  [PDFWidget](PDFWidget.md)

```javascript
var signatureWidget = pdfPage.createSignature("test")
```

### PDFPage.prototype.countAssociatedFiles()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return the number of [associated files](../../common/glossary.md#term-Associated-File)
associated with this page. Note that this is the number of
files associated with this page, not necessarily the
total number of files associated with elements throughout the
entire document.

* **Returns:**
  number

```javascript
var count = pdfPage.countAssociatedFiles()
```

### PDFPage.prototype.associatedFile(n)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return the [file specification](../../common/glossary.md#term-File-specification) object that represents the nth
Associated File on this page.

`n` should be in the range `0 <= n < countAssociatedFiles()`.

Returns null if no associated file exists or index is out of range.

* **Returns:**
  [PDFObject](PDFObject.md) | null

```javascript
var obj = pdfPage.associatedFile(0)
```
