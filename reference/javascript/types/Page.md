# Page

A Page object is a page that has been loaded from a [Document](Document.md).

Page objects that belong to a [PDFDocument](PDFDocument.md), also provide
the interface described in [PDFPage](PDFPage.md).

## Constructors

### *class* Page()

*You cannot create instances of this class with the new operator!*

Page instances are returned by [`Document.prototype.loadPage()`](Document.md#Document.prototype.loadPage).

## Constants

The [page box](../../common/glossary.md#term-Page-Box) types:

### Page.MEDIA_BOX

### Page.CROP_BOX

### Page.BLEED_BOX

### Page.TRIM_BOX

### Page.ART_BOX

## Instance methods

### Page.prototype.getBounds(box)

Returns a rectangle describing the page dimensions.

* **Arguments:**
  * **box** (`string`) – Which [page box](../../common/glossary.md#term-Page-Box) to query.
* **Returns:**
  [Rect](Rect.md)

```javascript
var rect = page.getBounds()
```

### Page.prototype.run(device, transform)

Calls device functions for all the contents on the page, using the
specified transform.

The device can be one of the built-in devices ([DrawDevice](DrawDevice.md) and [DisplayListDevice](DisplayListDevice.md))
or a Javascript [Device](Device.md).

The matrix transforms coordinates from user space to device space.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device object.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.

```javascript
page.run(dev, mupdf.Matrix.identity)
```

### Page.prototype.runPageContents(device, transform)

This is the same as the [`Page.prototype.run()`](#Page.prototype.run) method above but it only
runs the page itself and omits annotations and widgets.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device object.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.

```javascript
page.runPageContents(dev, mupdf.Matrix.identity)
```

### Page.prototype.runPageAnnots(device, transform)

This is the same as the [`Page.prototype.run()`](#Page.prototype.run) method above but it only
runs the page annotations.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device object.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.

```javascript
page.runPageAnnots(dev, mupdf.Matrix.identity)
```

### Page.prototype.runPageWidgets(device, transform)

This is the same as the [`Page.prototype.run()`](#Page.prototype.run) method above but it only
runs the page widgets.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device object.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.

```javascript
page.runPageWidgets(dev, mupdf.Matrix.identity)
```

### Page.prototype.toPixmap(matrix, colorspace, alpha, showExtras)

Render the page into a [Pixmap](Pixmap.md) using the specified transform
matrix and colorspace. If `alpha` is `true`, the page will be drawn
on a transparent background, otherwise white. If `showExtras` is
`true` then the operation will include any page annotations and/or
widgets.

* **Arguments:**
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The desired colorspace of the returned pixmap.
  * **alpha** (`boolean`) – Whether the resulting pixmap should have an alpha component. Defaults to `true`.
  * **showExtras** (`boolean`) – Whether to render annotations and widgets. Defaults to `true`.
* **Returns:**
  [Pixmap](Pixmap.md)

```javascript
var pixmap = page.toPixmap(mupdf.Matrix.identity, mupdf.ColorSpace.DeviceRGB, true, true)
```

### Page.prototype.toDisplayList(showExtras)

Record the contents on the page into a [DisplayList](DisplayList.md). If
`showExtras` is `true` then the operation will include all
annotations and/or widgets on the page.

* **Arguments:**
  * **showExtras** (`boolean`) – Whether to render annotations and widgets. Defaults to `true`.
* **Returns:**
  [DisplayList](DisplayList.md)

```javascript
var displayList = page.toDisplayList(true)
```

### Page.prototype.toStructuredText(options)

Extract the text on the page into a [StructuredText](StructuredText.md) object.

* **Arguments:**
  * **options** (`string`) – See [Structured Text Options](../../common/stext-options.md).
* **Returns:**
  [StructuredText](StructuredText.md)

```javascript
var sText = page.toStructuredText("preserve-whitespace")
```

### Page.prototype.search(needle, maxHits)

Search the page text for all instances of the `needle` value,
and return an array of search hits.

Each search hit is an array of [Quad](Quad.md), each corresponding
to a character in the search hit.

* **Arguments:**
  * **needle** (`string`) – The text to search for.
  * **maxHits** (`number`) – Maximum number of hits to return.
* **Returns:**
  Array of Array of [Quad](Quad.md)

```javascript
var results = page.search("my search phrase")
```

### Page.prototype.getLinks()

Return an array of all the links on the page. If there are no
links then an empty array is returned.

Each link is an object with a ‘bounds’ property, and either a
‘page’ or ‘uri’ property, depending on whether it’s an internal or
external link.

* **Returns:**
  Array of [Link](Link.md)

```javascript
var links = page.getLinks()
var link = links[0]
var linkDestination = doc.resolveLink(link)
```

### Page.prototype.createLink(rect, uri)

Create a new link with the supplied metrics for the page, linking to the destination URI string.

To create links to other pages within the document see the [`Document.prototype.formatLinkURI`](Document.md#Document.prototype.formatLinkURI) method.

* **Arguments:**
  * **rect** ([`Rect`](Rect.md#Rect)) – Rectangle specifying the active area on the page the link should cover.
  * **destinationUri** (`string`) – A URI string describing the desired link destination.
* **Returns:**
  [Link](Link.md).

```javascript
// create a link to an external URL
var link = page.createLink([0, 0, 100, 50], "https://example.com")

// create a link to another page in the document
var link = page.createLink([0, 100, 100, 150], "#page=1&view=FitV,0")
```

### Page.prototype.deleteLink(link)

Delete the link from the page.

* **Arguments:**
  * **link** ([`Link`](Link.md#Link)) – The link to remove.

```javascript
page.deleteLink(link_obj)
```

### Page.prototype.getLabel()

Returns the page number as a string using the numbering scheme of the document.

* **Returns:**
  string

```javascript
var label = page.getLabel()
```

### Page.prototype.isPDF()

Returns `true` if the page is from a PDF document.

* **Returns:**
  boolean

```javascript
var isPDF = page.isPDF()
```

### Page.prototype.decodeBarcode(subarea, rotate)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Decodes a barcode detected on the page, and returns an object with
properties for barcode type and contents.

* **Arguments:**
  * **subarea** ([`Rect`](Rect.md#Rect)) – Only detect barcode within subarea. Defaults to the entire page.
  * **rotate** (`number`) – Degrees of rotation to rotate page before detecting barcode. Defaults to 0.
* **Returns:**
  Object with barcode information.

```javascript
var info = page.decodeBarcode(page.getBounds(), 0)
```
