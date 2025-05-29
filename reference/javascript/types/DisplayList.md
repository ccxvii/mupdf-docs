# DisplayList

A display list is a sequence of device calls that can be replayed multiple
times. This is useful e.g. when you need to render a page at multiple
resolutions, or when you want to both render a page and later search for
text in it. Using a display list to do this, improves performance because
it avoids repeatedly reinterpreting the document from file

To populate a display list use the [DisplayListDevice](DisplayListDevice.md).

```javascript
var list = new mupdf.DisplayList([0, 0, 595, 842])
var listDevice = new mupdf.DisplayListDevice(list)
page.run(listDevice, mupdf.Matrix.identity)

var pixmap = new mupdf.Pixmap(mupdf.ColorSpace.DeviceRGB, [0, 0, 595, 842], false)
var drawDevice = new mupdf.DrawDevice(mupdf.Matrix.identity, pixmap)
list.run(drawDevice, mupdf.Matrix.identity)

var searchHits = list.search("hello world")
```

## Constructors

### *class* DisplayList(mediabox)

Create an empty display list. The mediabox rectangle should be the
bounds of the page.

* **Arguments:**
  * **mediabox** ([`Rect`](Rect.md#Rect)) – The size of the page.

```javascript
var displayList = new mupdf.DisplayList([0, 0, 595, 842])
```

## Instance methods

### DisplayList.prototype.run(device, matrix)

Play back this display lists sequence of device calls to the given device.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device to replay the device calls to.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – Transformation matrix to apply to coordinates in all device calls.

```javascript
displayList.run(device, mupdf.Matrix.identity)
```

### DisplayList.prototype.getBounds()

Return a bounding rectangle that encompasses all the contents of the display list.

* **Returns:**
  [Rect](Rect.md)

```javascript
var bounds = displayList.getBounds()
```

### DisplayList.prototype.toPixmap(matrix, colorspace, alpha)

Render a display list to a [Pixmap](Pixmap.md).

* **Arguments:**
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – Transformation matrix.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The desired colorspace of the returned pixmap.
  * **alpha** (`boolean`) – Whether the returned pixmap has transparency or not. If the pixmap handles transparency, it starts out transparent (otherwise it is filled white), before the contents of the display list are rendered onto the pixmap.
* **Returns:**
  [Pixmap](Pixmap.md)

```javascript
var pixmap = displayList.toPixmap(mupdf.Matrix.identity, mupdf.ColorSpace.DeviceRGB, false)
```

### DisplayList.prototype.toStructuredText(options)

Extract the text on the page into a [StructuredText](StructuredText.md) object.

* **Arguments:**
  * **options** (`string`) – See [Structured Text Options](../../common/stext-options.md).
* **Returns:**
  [StructuredText](StructuredText.md)

```javascript
var sText = displayList.toStructuredText("preserve-whitespace")
```

### DisplayList.prototype.search(needle, max_hits)

Search the display list text for all instances of the text value
`needle`, and return an array of search hits. Each search hit is an
array of [Quad](Quad.md), each corresponding to a single character in the search
hit.

* **Arguments:**
  * **needle** (`string`) – The text to search for.
  * **max_hits** (`number`) – Set to limit number of results, defaults to 500.
* **Returns:**
  Array of Array of [Quad](Quad.md)

```javascript
var results = displayList.search("my search phrase")
```

### DisplayList.protoype.decodeBarcode(subarea, rotate)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Decodes a barcode detected in the display list, and returns an
object with properties for barcode type and contents.

* **Arguments:**
  * **subarea** ([`Rect`](Rect.md#Rect)) – Only detect barcode within subarea. Defaults to the entire area returned by [`DisplayList.prototype.getBounds()`](#DisplayList.prototype.getBounds).
  * **rotate** (`number`) – Degrees of rotation to rotate display list before detecting barcode. Defaults to 0.
* **Returns:**
  Object with barcode information.

```javascript
var barcodeInfo = displayList.decodeBarcode([0, 0, 100, 100 ], 0)
```
