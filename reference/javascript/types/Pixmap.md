# Pixmap

A Pixmap object contains a color raster image (short for pixel map).
The components in a pixel in the Pixmap are all byte values,
with the transparency as the last component.

A Pixmap also has a location (x, y) in addition to its size;
so that they can easily be used to represent tiles of a page.

## Constructors

### *class* Pixmap(colorspace, bbox, alpha)

### *class* Pixmap(pixmap, mask)

Create a new empty Pixmap whose pixel data is **not**
initialized. Alternatively create a new Pixmap based on an
existing Pixmap without alpha and combine it with a single
component soft mask of the same dimensions.

* **Arguments:**
  * **colorspace** (`ColorSpace | null`) – The desired colorspace for the new pixmap. `null` implies a single component alpha pixmap.
  * **bbox** ([`Rect`](Rect.md#Rect)) – The desired dimensions of the new pixmap.
  * **alpha** (`boolean`) – Whether the new pixmap should have an alpha component.
  * **pixmap** ([`Pixmap`](#Pixmap)) – The original pixmap without alpha.
  * **mask** ([`Pixmap`](#Pixmap)) – Soft mask used as alpha in the combined pixmap.

```javascript
var pixmap1 = new mupdf.Pixmap(mupdf.ColorSpace.DeviceRGB, [0, 0, 100, 100], true)
var pixmap2 = new mupdf.Pixmap(
        new mupdf.Image("photo.png").toPixmap(),
        new mupdf.Image("softmask.png").toPixmap()
)
```

## Instance methods

### Pixmap.prototype.clear(value)

Clear the pixels to the specified value. Pass 255 for white, 0 for black, or omit for transparent.

* **Arguments:**
  * **value** (`number`) – The value to use for clearing.

```javascript
pixmap.clear(255)
```

### Pixmap.prototype.getBounds()

Return the pixmap bounds.

* **Returns:**
  [Rect](Rect.md)

```javascript
var rect = pixmap.getBounds()
```

### Pixmap.prototype.getWidth()

Get the width of the pixmap.

* **Returns:**
  number

```javascript
var w = pixmap.getWidth()
```

### Pixmap.prototype.getHeight()

Get the height of the pixmap.

* **Returns:**
  number

```javascript
var h = pixmap.getHeight()
```

### Pixmap.prototype.getNumberOfComponents()

Number of colors; plus one if an alpha channel is present.

* **Returns:**
  number

```javascript
var num = pixmap.getNumberOfComponents()
```

### Pixmap.prototype.getAlpha()

Returns whether an alpha channel is present.

* **Returns:**
  boolean

```javascript
var alpha = pixmap.getAlpha()
```

### Pixmap.prototype.getStride()

Number of bytes per row.

* **Returns:**
  number

```javascript
var stride = pixmap.getStride()
```

### Pixmap.prototype.getColorSpace()

Returns the colorspace of this pixmap. Returns null if the pixmap has
no colors (for example if it is an opacity mask with only an alpha
channel).

* **Returns:**
  [ColorSpace](ColorSpace.md) | null

```javascript
var cs = pixmap.getColorSpace()
```

### Pixmap.prototype.setResolution(x, y)

Set horizontal and vertical resolution.

* **Arguments:**
  * **x** (`number`) – Horizontal resolution in dots per inch.
  * **y** (`number`) – Vertical resolution in dots per inch.

```javascript
pixmap.setResolution(300, 300)
```

### Pixmap.prototype.getX()

Returns the x coordinate of the pixmap.

```javascript
var x = pixmap.getX()
```

### Pixmap.prototype.getY()

Returns the y coordinate of the pixmap.

* **Returns:**
  number

```javascript
var y = pixmap.getY()
```

### Pixmap.prototype.getXResolution()

Returns the horizontal resolution in dots per inch for this pixmap.

* **Returns:**
  number

```javascript
var xRes = pixmap.getXResolution()
```

### Pixmap.prototype.getYResolution()

Returns the vertical resolution in dots per inch for this pixmap.

* **Returns:**
  number

```javascript
var yRes = pixmap.getYResolution()
```

### Pixmap.prototype.invert()

Invert all pixels. All components are processed, except alpha which is unchanged.

```javascript
pixmap.invert()
```

### Pixmap.prototype.invertLuminance()

Transform all pixels so that luminance of each pixel is inverted,
and the chrominance remains as unchanged as possible.
All components are processed, except alpha which is unchanged.

```javascript
pixmap.invertLuminance()
```

### Pixmap.prototype.gamma(p)

Apply gamma correction to this pixmap. All components are processed,
except alpha which is unchanged.

Values `>= 0.1 & < 1` darkens the pixmap, `> 1 & < 10` lightens the pixmap.

* **Arguments:**
  * **p** (`number`) – Desired gamma level.

```javascript
pixmap.gamma(3.5)
```

### Pixmap.prototype.tint(black, white)

Tint all pixels in RGB, BGR or Gray pixmaps.
Map black and white respectively to the given hex RGB values.

* **Arguments:**
  * **black** (`Color | number`) – Map black to this color.
  * **white** (`Color | number`) – Map white to this color.

```javascript
pixmap.tint(0xffff00, 0xffff00)
```

### Pixmap.prototype.warp(points, width, height)

Return a warped subsection of this pixmap, where the corner of
the input quadrilateral will be “warped” to become the four corner
points of the returned pixmap defined by the requested dimensions.

* **Arguments:**
  * **points** ([`Quad`](Quad.md#Quad)) – The corners of a convex quadrilateral within the [Pixmap]() to be warped.
  * **width** (`number`) – Width of resulting pixmap.
  * **height** (`number`) – Height of resulting pixmap.
* **Returns:**
  [Pixmap]()

```javascript
var warpedPixmap = pixmap.warp([[0, 0], [100, 100], [130, 170], [150, 200]], 200, 200)
```

### Pixmap.prototype.autowarp(points)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Same as [`Pixmap.prototype.warp()`](#Pixmap.prototype.warp) except that width and height
are automatically determined.

* **Arguments:**
  * **points** ([`Quad`](Quad.md#Quad)) – The corners of a convex quadrilateral within the [Pixmap]() to be warped.
* **Returns:**
  [Pixmap]()

```javascript
var warpedPixmap = pixmap.autowarp([0,0,100,0,0,100,100,100])
```

<!-- TODO murun has Pixmap.prototype.convertToColorSpace(colorspace, proofCS, defaultCS, colorParams, keepAlpha), not sure how to reconcile the docs. I could put keepAlpha as the second argument and just leave out the rest. thoughts? -->

### Pixmap.prototype.convertToColorSpace(colorspace, keepAlpha)

Convert pixmap into a new pixmap of a desired colorspace.
A proofing colorspace, a set of default colorspaces and color
parameters used during conversion may be specified.
Finally a boolean indicates if alpha should be preserved
(default is to not preserve alpha).

* **Arguments:**
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The desired colorspace.
  * **keepAlpha** (`boolean`) – Whether to keep the alpha component.
* **Returns:**
  [Pixmap]()

### Pixmap.prototype.getPixels()

Returns an array of pixels for this pixmap.

* **Returns:**
  Array of number

```javascript
var pixels = pixmap.getPixels()
```

### Pixmap.prototype.asPNG()

Returns a buffer of this pixmap as a PNG.

* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = pixmap.asPNG()
```

### Pixmap.prototype.asPSD()

Returns a buffer of this pixmap as a PSD.

* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = pixmap.asPSD()
```

### Pixmap.prototype.asPAM()

Returns a buffer of this pixmap as a PAM.

* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = pixmap.asPAM()
```

### Pixmap.prototype.asJPEG(quality, invert_cmyk)

Returns a buffer of this pixmap as a JPEG.
Note, if this pixmap has an alpha channel then an exception will be thrown.

* **Arguments:**
  * **quality** (`number`) – Desired compression quality, between `0` and `100`.
  * **invert_cmyk** (`boolean`) – How to handle polarity in [CMYK JPEG](../../common/glossary.md#term-CMYK-JPEG) images.
* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = pixmap.asJPEG(80, false)
```

### Pixmap.prototype.decodeBarcode(rotate)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Decodes a barcode detected in the pixmap, and returns an object with
properties for barcode type and contents.

* **Arguments:**
  * **rotate** (`number`) – Degrees of rotation to rotate pixmap before detecting barcode. Defaults to 0.
* **Returns:**
  Object with barcode information.

```javascript
var barcodeInfo = displayList.decodeBarcode([0, 0, 100, 100 ], 0)
```

### Pixmap.prototype.encodeBarcode(barcodeType, contents, size, errorCorrectionLevel, quietZones, humanReadableText)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Encodes a barcode into a pixmap. The supported types of barcode is either one of:

|               |               |                  | Matrix          | Linear Product    | Linear Industrial   |
|---------------|---------------|------------------|-----------------|-------------------|---------------------|
| String        | Name          | String           | Name            | String            | Name                |
| `qrcode`      | QR Code       | `upca`           | UPC-A           | `code39`          | Code 39             |
| `microqrcode` | Micro QR Code | `upce`           | UPC-E           | `code93`          | Code 93             |
| `rmqrcode`    | rMQR Code     | `ean8`           | EAN-8           | `code128`         | Code 128            |
| `aztec`       | Aztec         | `ean13`          | EAN-13          | `codabar`         | Codabar             |
| `datamatrix`  | DataMatrix    | `databar`        | DataBar         | `databarexpanded` | DataBar Expanded    |
| `pdf417`      | PDF417        | `databarlimited` | DataBar Limited | `dxfilmedge`      | DX Film Edge        |
| `maxicode`    | MaxiCode      |                  |                 | `itf`             | ITF                 |
* **Arguments:**
  * **barcodeType** (`string`) – The desired barcode type.
  * **contents** (`string`) – The textual content to encode into the barcode.
  * **size** (`number`) – The size of the barcode in pixels.
  * **errorCorrectionLevel** (`number`) – The error correction level (0-8).
  * **quietZones** (`boolean`) – Whether to add an empty margin around the barcode.
  * **humanReadableText** (`boolean`) – Whether to add human-readable text. Some barcodes, e.g. EAN-13, can have the barcode contents printed in human-readable text next to the barcode.
* **Returns:**
  [Pixmap]()

```javascript
var pix = Pixmap.encodeBarcode("qrcode", "Hello world!", 100, 2, true, false)
```

### Pixmap.prototype.getSample(x, y, index)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Get the value of component `index` at position x, y (relative to
the image origin: 0, 0 is the top left pixel).

* **Arguments:**
  * **x** (`number`) – X coordinate.
  * **y** (`number`) – Y coordinate.
  * **index** (`number`) – Component index. i.e. For CMYK ColorSpaces 0 = Cyan, 3 = Black, for RGB 0 = Red, 2 == Blue etc.
* **Throws:**
  RangeError if x, y, or index are out of range.
* **Returns:**
  number

```javascript
// Get green component of pixel at 10, 10
var sample = rgbpixmap.getSample(10, 10, 1)
```

### Pixmap.prototype.saveAsPNG(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a PNG. Only works for gray and RGB images.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.

```javascript
pixmap.saveAsPNG("filename.png")
```

### Pixmap.prototype.saveAsJPEG(filename, quality)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a JPEG file. Only works for gray, RGB and CMYK images.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.
  * **quality** (`number`) – Desired quality between 0 and 100. Defaults to 90.

```javascript
pixmap.saveAsJPEG("filename.jpg", 80)
```

### Pixmap.prototype.saveAsPAM(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a PAM file.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.

```javascript
pixmap.saveAsPAM("filename.pam")
```

### Pixmap.prototype.saveAsPNM(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a PNM file. Only works for gray and RGB images without alpha.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.

```javascript
pixmap.saveAsPNM("filename.pnm")
```

### Pixmap.prototype.saveAsPBM(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a PBM file. Only works for alpha only, gray and CMYK images without alpha.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.

```javascript
pixmap.saveAsPBM("filename.pbm")
```

### Pixmap.prototype.saveAsPKM(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a PKM file. Only works for alpha only, gray and CMYK images without alpha.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.

```javascript
pixmap.saveAsPKM("filename.pkm")
```

### Pixmap.prototype.saveAsJPX(filename, quality)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save this Pixmap as a JPX file.

* **Arguments:**
  * **filename** (`string`) – Desired name of image file.
  * **quality** (`number`) – Desired quality between 0 and 100. Defaults to 90.

```javascript
pixmap.saveAsJPX("filename.jpx", 90)
```

### Pixmap.prototype.detectDocument(points)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Detect a “document” in a [Pixmap](). Only a grayscale [Pixmap]()
without alpha is supported, anything else will cause an exception
to be thrown.

Returns null if no document was detected.

* **Returns:**
  [Quad](Quad.md) | null

```javascript
var documentLocation = pixmap.detectDocument([0,0,100,0,100,100,0,100])
```

### Pixmap.prototype.detectSkew()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns the angle of skew detected from [Pixmap]().
Note, if the [Pixmap]() is not Greyscale with no alpha then an exception will be thrown.

* **Returns:**
  number

```javascript
var angle = pixmap.detectSkew()
```

### Pixmap.prototype.deskew(angle, border)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a new [Pixmap]() being the deskewed version of the supplied [Pixmap]().
Note, if a [Pixmap]() is supplied that is not RGB or Greyscale, or has alpha then an exception will be thrown.

* **Arguments:**
  * **angle** (`number`) – The angle to deskew.
  * **border** (`string`) – “increase” increases the size of the pixmap so no pixels are lost. “maintain” maintains the size of the pixmap. “decrease” decreases the size of the page so no new pixels are shown.
* **Returns:**
  [Pixmap]()

```javascript
var deskewed = pixmap.deskew(angle, 0)
```

### Pixmap.prototype.computeMD5()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns the MD5 digest of the pixmap pixel data.
The digest is returned as a string of 16 hex digits.

* **Returns:**
  string

```javascript
var md5 = pixmap.computeMD5()
```
