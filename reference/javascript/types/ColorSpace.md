# ColorSpace

Defines what color system is used for an object, and which colorants exist.

For a [Pixmap](Pixmap.md) the interface [`Pixmap.prototype.getColorSpace()`](Pixmap.md#Pixmap.prototype.getColorSpace) returns its colorspace,
which defines what colorants make up the pixel colors in the Pixmap and
how those colorants are stored. If that colorspace is e.g.,
[`ColorSpace.DeviceRGB`](#ColorSpace.DeviceRGB), then the colorants **red**, **green** and
**blue** are combined to make up the color of each pixel.

## Constructors

### *class* ColorSpace(from, name)

Create a new ColorSpace from an ICC profile.

* **Arguments:**
  * **from** (`Buffer | ArrayBuffer | Uint8Array | string`) – A buffer containing an ICC profile.
  * **name** (`string`) – A user descriptive name for the new colorspace.

```javascript
var icc_colorspace = new mupdf.ColorSpace(fs.readFileSync("SWOP.icc"), "SWOP")
```

## Static properties

### ColorSpace.DeviceGray

The default Grayscale colorspace

### ColorSpace.DeviceRGB

The default RGB colorspace

### ColorSpace.DeviceBGR

The default RGB colorspace, but with components in reverse order

### ColorSpace.DeviceCMYK

The default CMYK colorspace

### ColorSpace.Lab

The default Lab colorspace

## Instance methods

### ColorSpace.prototype.getName()

Return the name of this colorspace.

### ColorSpace.prototype.getNumberOfComponents()

A Grayscale colorspace has one component, RGB has 3, CMYK has 4, and DeviceN may have any number of components.

* **Returns:**
  number of components

```javascript
var cs = mupdf.ColorSpace.DeviceRGB
var num = cs.getNumberOfComponents() // 3
```

### ColorSpace.prototype.getType()

Returns a string indicating the type of this colorspace, one of:

* **Returns:**
  `"None" | "Gray" | "RGB" | "BGR" | "CMYK" | "Lab" | "Indexed" | "Separation"`

### ColorSpace.prototype.isCMYK()

Returns `true` if the object is a CMYK color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isCMYK()
```

### ColorSpace.prototype.isDeviceN()

Returns `true` if the object is a Device N color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isDeviceN()
```

### ColorSpace.prototype.isGray()

Returns `true` if the object is a gray color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isGray()
```

### ColorSpace.prototype.isIndexed()

Returns `true` if the object is an Indexed color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isIndexed()
```

### ColorSpace.prototype.isLab()

Returns `true` if the object is a Lab color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isLab()
```

### ColorSpace.prototype.isRGB()

Returns `true` if the object is an RGB color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isRGB()
```

### ColorSpace.prototype.isSubtractive()

Returns `true` if the object is a subtractive color space.

* **Returns:**
  boolean

```javascript
var bool = colorSpace.isSubtractive()
```

### ColorSpace.prototype.toString()

Return name of this colorspace.

* **Returns:**
  string

```javascript
var cs = mupdf.ColorSpace.DeviceRGB
var name = cs.toString() // "[ColorSpace DeviceRGB]"
```
