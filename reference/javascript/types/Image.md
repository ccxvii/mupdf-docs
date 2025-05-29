# Image

## Constructors

### *class* Image(pixmap, mask)

### *class* Image(data, mask)

### *class* Image(filename, mask)

Create an Image and populate it with image data decoded either from a pixmap,
a data buffer, or a file.

* **Arguments:**
  * **pixmap** ([`Pixmap`](Pixmap.md#Pixmap)) – A pixmap with the image data.
  * **data** (`Buffer | ArrayBuffer | Uint8Array`) – Image data.
  * **filename** (`string`) – Image file to load.
  * **mask** ([`Image`](#Image)) – An optional image mask.

```javascript
var image1 = new mupdf.Image(pixmap, imagemask)
var image2 = new mupdf.Image(buffer, imagemask)
var image3 = new mupdf.Image("logo.png", imagemask)
```

## Instance methods

### Image.prototype.getWidth()

Get the image width in pixels.

* **Returns:**
  number

```javascript
var width = image.getWidth()
```

### Image.prototype.getHeight()

Get the image height in pixels.

* **Returns:**
  number

```javascript
var height = image.getHeight()
```

### Image.prototype.getXResolution()

Returns the x resolution for the [Image]() in dots per inch.

* **Returns:**
  number

```javascript
var xRes = image.getXResolution()
```

### Image.prototype.getYResolution()

Returns the y resolution for the [Image]() in dots per inch.

* **Returns:**
  number

```javascript
var yRes = image.getYResolution()
```

### Image.prototype.getColorSpace()

Returns the [ColorSpace](ColorSpace.md) for the [Image](). Returns null if the image has
no colors (for example if it is an opacity mask with only an alpha
channel).

* **Returns:**
  [ColorSpace](ColorSpace.md) | null

```javascript
var cs = image.getColorSpace()
```

### Image.prototype.getNumberOfComponents()

Number of colors; plus one if an alpha channel is present.

* **Returns:**
  number

```javascript
var num = image.getNumberOfComponents()
```

### Image.prototype.getBitsPerComponent()

Returns the number of bits per component.

* **Returns:**
  number

```javascript
var bits = image.getBitsPerComponent()
```

### Image.prototype.getImageMask()

Returns `true` if this image is an image mask.

* **Returns:**
  boolean

```javascript
var hasMask = image.getImageMask()
```

### Image.prototype.getMask()

Get another [Image]() used as a mask for this one.

* **Returns:**
  [Image]() | null

```javascript
var mask = image.getMask()
```

### Image.prototype.getInterpolate()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns whether interpolation was used during decoding.

* **Returns:**
  boolean

```javascript
var interpolate = image.getInterpolate()
```

### Image.prototype.getColorKey()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns an array with `2 * getNumberOfComponents()` integers
for an image with color key masking, or `null` if masking is
not used. Each pair of integers define an interval, and
component values within those intervals are not painted.

* **Returns:**
  Array of number | null

```javascript
var result = image.getColorKey()
```

### Image.prototype.getDecode()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns an array with `2 * getNumberOfComponents()` numbers
for an image with color mapping, or `null` if mapping is not
used. Each pair of numbers define the lower and upper values to
which the component values are mapped linearly.

* **Returns:**
  Array of number | null

```javascript
var arr = image.getDecode()
```

### Image.prototype.getOrientation()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns the orientation of the image.

* **Returns:**
  number

```javascript
var orientation = image.getOrientation()
```

### Image.prototype.setOrientation(orientation)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Set the image orientation to the given orientation.

* **Arguments:**
  * **orientation** (`number`) – 

    Orientation value described in this table:

    |   Value | Description                                           |
    |---------|-------------------------------------------------------|
    |       0 | Undefined                                             |
    |       1 | 0 degree ccw rotation. (Exif = 1)                     |
    |       2 | 90 degree ccw rotation. (Exit = 8)                    |
    |       3 | 180 degree ccw rotation. (Exif = 3)                   |
    |       4 | 270 degree ccw rotation. (Exif = 6)                   |
    |       5 | flip on X. (Exif = 2)                                 |
    |       6 | flip on X, then rotate ccw by 90 degrees. (Exif = 5)  |
    |       7 | flip on X, then rotate ccw by 180 degrees. (Exif = 4) |
    |       4 | flip on X, then rotate ccw by 270 degrees. (Exif = 7) |

### Image.prototype.toPixmap(scaledWidth, scaledHeight)

Create a [Pixmap](Pixmap.md) from this image. The `scaledWidth` and
`scaledHeight` arguments are optional, but may be used to decode a
down-scaled [Pixmap](Pixmap.md).

* **Arguments:**
  * **scaledWidth** (`number`)
  * **scaledHeight** (`number`)
* **Returns:**
  [Pixmap](Pixmap.md)

```javascript
var pixmap = image.toPixmap()
var scaledPixmap = image.toPixmap(100, 100)
```
