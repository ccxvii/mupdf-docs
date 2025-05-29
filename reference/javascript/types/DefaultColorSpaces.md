# DefaultColorSpaces

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

An object given to a [Device](Device.md) with the [`Device.prototype.setDefaultColorSpaces()`](Device.md#Device.prototype.setDefaultColorSpaces) call so it
can obtain the current default [ColorSpace](ColorSpace.md) objects.

## Constructors

### *class* DefaultColorSpaces()

## Instance methods

### DefaultColorSpaces.prototype.getDefaultGray()

Get the default gray colorspace.

* **Returns:**
  [ColorSpace](ColorSpace.md)

### DefaultColorSpaces.prototype.getDefaultRGB()

Get the default RGB colorspace.

* **Returns:**
  [ColorSpace](ColorSpace.md)

### DefaultColorSpaces.prototype.getDefaultCMYK()

Get the default CMYK colorspace.

* **Returns:**
  [ColorSpace](ColorSpace.md)

### DefaultColorSpaces.prototype.getOutputIntent()

Get the output intent.

* **Returns:**
  [ColorSpace](ColorSpace.md)

### DefaultColorSpaces.prototype.setDefaultGray(colorspace)

* **Arguments:**
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The new default gray colorspace.

### DefaultColorSpaces.prototype.setDefaultRGB(colorspace)

* **Arguments:**
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The new default RGB  colorspace.

### DefaultColorSpaces.prototype.setDefaultCMYK(colorspace)

* **Arguments:**
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The new default CMYK colorspace.

### DefaultColorSpaces.prototype.setOutputIntent(colorspace)

* **Arguments:**
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The new default output intent.
