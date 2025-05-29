# Device

All built-in devices have the methods listed below. Any function that
accepts a device will also accept a Javascript object with the same
methods. Any missing methods are simply ignored, so you only need to
create methods for the device calls you care about.

Many of the methods take graphics objects as arguments: [Path](Path.md),
[Text](Text.md), [Image](Image.md) and [Shade](Shade.md).

Colors are specified as arrays with the appropriate number of components
for the color space.

The methods that clip graphics, e.g. [`Device.prototype.clipPath()`](#Device.prototype.clipPath),
[`Device.prototype.clipStrokePath()`](#Device.prototype.clipStrokePath), [`Device.prototype.clipText()`](#Device.prototype.clipText), etc., must be balanced with
a corresponding [`Device.prototype.popClip()`](#Device.prototype.popClip).

## Constructors

### *class* Device(callbacks)

Create a Device which calls back to Javascript.

The callback object may provide functions matching the methods
on the Device class. Any device calls that don’t have a corresponding
function will simply be ignored.

* **Arguments:**
  * **callbacks** – object containing callback functions

You can create other devices with [DrawDevice](DrawDevice.md) and [DisplayListDevice](DisplayListDevice.md).

## Constants

[Blend mode](../../common/glossary.md#term-Blend-Mode) constants for use with [`Device.prototype.beginGroup`](#Device.prototype.beginGroup):

### Device.BLEND_NORMAL

### Device.BLEND_MULTIPLY

### Device.BLEND_SCREEN

### Device.BLEND_OVERLAY

### Device.BLEND_DARKEN

### Device.BLEND_LIGHTEN

### Device.BLEND_COLOR_DODGE

### Device.BLEND_COLOR_BURN

### Device.BLEND_HARD_LIGHT

### Device.BLEND_SOFT_LIGHT

### Device.BLEND_DIFFERENCE

### Device.BLEND_EXCLUSION

### Device.BLEND_HUE

### Device.BLEND_SATURATION

### Device.BLEND_COLOR

### Device.BLEND_LUMINOSITY

## Instance methods

### Device.prototype.close()

Tell this device that we are done, and flush any pending output.

Before closing, ensure that there have been as many calls to
[`popClip()`](#Device.prototype.popClip) as there have been to the clipping functions:
[`clipPath()`](#Device.prototype.clipPath), [`clipStrokePath()`](#Device.prototype.clipStrokePath), [`clipText()`](#Device.prototype.clipText), etc.

```javascript
device.close()
```

### Line art

### Device.prototype.fillPath(path, evenOdd, ctm, colorspace, color, alpha)

Fill a path.

* **Arguments:**
  * **path** ([`Path`](Path.md#Path)) – Path object.
  * **evenOdd** (`boolean`) – Use [even-odd rule](../../common/glossary.md#term-Even-Odd-Rule) or [non-zero winding number rule](../../common/glossary.md#term-Non-zero-Winding-Number-Rule) to fill the path.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – The colorspace of the color to fill with.
  * **color** (`Color`) – The color to fill the path with.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.fillPath(path, false, mupdf.Matrix.identity, mupdf.ColorSpace.DeviceRGB, [1, 0, 0], true)
```

### Device.prototype.strokePath(path, stroke, ctm, colorspace, color, alpha)

Stroke a path.

* **Arguments:**
  * **path** ([`Path`](Path.md#Path)) – Path object.
  * **stroke** ([`StrokeState`](StrokeState.md#StrokeState)) – Stroke state.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace.
  * **color** (`Color`) – The color to stroke the path with.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.strokePath(path,
        {dashes: [5, 10], lineWidth: 3, lineCap: 'Round' },
        mupdf.Matrix.identity,
        mupdf.ColorSpace.DeviceRGB,
        [0, 1, 0],
        0.5
)
```

### Device.prototype.clipPath(path, evenOdd, ctm)

Clip a path.

* **Arguments:**
  * **path** ([`Path`](Path.md#Path)) – Path object.
  * **evenOdd** (`boolean`) – Use [even-odd rule](../../common/glossary.md#term-Even-Odd-Rule) or [non-zero winding number rule](../../common/glossary.md#term-Non-zero-Winding-Number-Rule) to fill the path.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.clipPath(path, true, mupdf.Matrix.identity)
```

### Device.prototype.clipStrokePath(path, stroke, ctm)

Clip & stroke a path.

* **Arguments:**
  * **path** ([`Path`](Path.md#Path)) – Path object.
  * **stroke** ([`StrokeState`](StrokeState.md#StrokeState)) – Stroke state.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.clipStrokePath(path, true, mupdf.Matrix.identity)
```

### Text

### Device.prototype.fillText(text, ctm, colorspace, color, alpha)

Fill a text object.

* **Arguments:**
  * **text** ([`Text`](Text.md#Text)) – Text object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace
  * **color** (`Color`) – The color used to fill the text.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.fillText(text, mupdf.Matrix.identity, mupdf.ColorSpace.DeviceRGB, [1, 0, 0], 1)
```

### Device.prototype.strokeText(text, stroke, ctm, colorspace, color, alpha)

Stroke a text object.

* **Arguments:**
  * **text** ([`Text`](Text.md#Text)) – Text object.
  * **stroke** ([`StrokeState`](StrokeState.md#StrokeState)) – Stroke state.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace
  * **color** (`Color`) – The color used to stroke the text.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.strokeText(text,
        { dashes: [5, 10], lineWidth: 3, lineCap: 'Round' },
        mupdf.Matrix.identity,
        mupdf.ColorSpace.DeviceRGB,
        [1, 0, 0],
        1
)
```

### Device.prototype.clipText(text, ctm)

Clip a text object.

* **Arguments:**
  * **text** ([`Text`](Text.md#Text)) – Text object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.clipText(text, mupdf.Matrix.identity)
```

### Device.prototype.clipStrokeText(text, stroke, ctm)

Clip & stroke a text object.

* **Arguments:**
  * **text** ([`Text`](Text.md#Text)) – Text object.
  * **stroke** ([`StrokeState`](StrokeState.md#StrokeState)) – stroke state.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.clipStrokeText(text,
        { dashes: [5, 10], lineWidth: 3, lineCap: 'Round' },
        mupdf.Matrix.identity
)
```

### Device.prototype.ignoreText(text, ctm)

Invisible text that can be searched but should not be visible, such as for overlaying a scanned OCR image.

* **Arguments:**
  * **text** ([`Text`](Text.md#Text)) – Text object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.ignoreText(text, mupdf.Matrix.identity)
```

### Shadings

### Device.prototype.fillShade(shade, ctm, alpha)

Fill a shading, also known as a gradient.

* **Arguments:**
  * **shade** ([`Shade`](Shade.md#Shade)) – The gradient.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.fillShade(shade, mupdf.Matrix.identity, true, { overPrinting: true })
```

### Images

### Device.prototype.fillImage(image, ctm, alpha)

Draw an image. An image always fills a unit rectangle [0, 0, 1, 1], so must be transformed to be placed and drawn at the appropriate size.

* **Arguments:**
  * **image** ([`Image`](Image.md#Image)) – Image object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.fillImage(image, mupdf.Matrix.identity, false, { overPrinting: true })
```

### Device.prototype.fillImageMask(image, ctm, colorspace, color, alpha)

An image mask is an image without color. Fill with the color where the image is opaque.

* **Arguments:**
  * **image** ([`Image`](Image.md#Image)) – Image object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace
  * **color** (`Color`) – The color to be used.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

```javascript
device.fillImageMask(image, mupdf.Matrix.identity, mupdf.ColorSpace.DeviceRGB, [0, 1, 0], true)
```

### Device.prototype.clipImageMask(image, ctm)

Clip graphics using the image to mask the areas to be drawn.

* **Arguments:**
  * **image** ([`Image`](Image.md#Image)) – Image object.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.

```javascript
device.clipImageMask(image, mupdf.Matrix.identity)
```

### Clipping and masking

### Device.prototype.popClip()

Pop the clip mask installed by the last clipping operation.

```javascript
device.popClip()
```

### Device.prototype.beginMask(area, luminosity, colorspace, color)

Create a soft mask. Any drawing commands between [`beginMask`](#Device.prototype.beginMask) and [`endMask`](#Device.prototype.endMask) are grouped and used as a clip mask.

* **Arguments:**
  * **area** ([`Rect`](Rect.md#Rect)) – Mask area.
  * **luminosity** (`boolean`) – If luminosity is `true`, the mask is derived from the luminosity (grayscale value) of the graphics drawn; otherwise the color is ignored completely and the mask is derived from the alpha of the group.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace
  * **color** (`Color`) – The color to be used.

```javascript
device.beginMask([0, 0, 100, 100], true, mupdf.ColorSpace.DeviceRGB, [1, 0, 1])
```

### Device.prototype.endMask()

Ends the mask.

```javascript
device.endMask()
```

### Groups and transparency

### Device.prototype.beginGroup(area, colorspace, isolated, knockout, blendmode, alpha)

Begin a transparency blending group. See [knockout and isolation](../../common/glossary.md#term-Knockout-and-Isolation)
and [blend mode](../../common/glossary.md#term-Blend-Mode) in the glossary for a cursory overview of the
concepts.

* **Arguments:**
  * **area** ([`Rect`](Rect.md#Rect)) – The blend area.
  * **colorspace** ([`ColorSpace`](ColorSpace.md#ColorSpace)) – Colorspace
  * **isolated** (`boolean`) – Whether the group is isolated.
  * **knockout** (`boolean`) – Whether the group is knockout.
  * **blendmode** (`string`) – The blend mode used when compositing this group with its backdrop.
  * **alpha** (`number`) – The [opacity](../../common/glossary.md#term-Opacity).

The blendmode is one of these string values or the corresponding enum constants:
Normal, Multiply, Screen, Overlay, Darken, Lighten, ColorDodge, ColorBurn, HardLight, SoftLight, Difference, Exclusion, Hue, Saturation, Color, Luminosity.

You can also use the [`Device.BLEND_NORMAL`](#Device.BLEND_NORMAL) constant:

```javascript
device.beginGroup(
        [0, 0, 100, 100],
        mupdf.ColorSpace.DeviceRGB,
        true,
        true,
        "Multiply",
        0.5
)
```

### Device.prototype.endGroup()

Ends the blending group.

```javascript
device.endGroup()
```

### Tiling

### Device.prototype.beginTile(area, view, xstep, ystep, ctm, id)

Draw a tiling pattern. Any drawing commands between [`beginTile`](#Device.prototype.beginTile) and [`endTile`](#Device.prototype.endTile) are grouped and then repeated across the whole page. Apply a clip mask to restrict the pattern to the desired shape.

* **Arguments:**
  * **area** ([`Rect`](Rect.md#Rect)) – Area
  * **view** ([`Rect`](Rect.md#Rect)) – View
  * **xstep** (`number`) – x step.
  * **ystep** (`number`) – y step.
  * **ctm** ([`Matrix`](Matrix.md#Matrix)) – The transform to apply.
  * **id** (`number`) – The purpose of id is to allow for efficient caching of rendered
    tiles. If id is 0, then no caching is performed. If it is
    non-zero, then it assumed to uniquely identify this tile.

```javascript
device.beginTile([0, 0, 100, 100], [100, 100, 200, 200], 10, 10, mupdf.Matrix.identity, 0)
```

### Device.prototype.endTile()

Ends the tiling pattern.

```javascript
device.endTile()
```

### Render flags

### Device.prototype.renderFlags(set, clear)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

The specified rendering flags are set, and some others are cleared.

Both set and clear are arrays where each element one of these flag names:

- “mask”
- “color”
- “uncacheable”
- “fillcolor-undefined”
- “strokecolor-undefined”
- “startcap-undefined”
- “dashcap-undefined”
- “endcap-undefined”
- “linejoin-undefined”
- “miterlimit-undefined”
- “linewidth-undefined”
- “bbox-defined”
- “gridfit-as-tiled”

* **Arguments:**
  * **set** (`Array of string`) – Rendering flags to set.
  * **clear** (`Array of string`) – Rendering flags to clear.

```javascript
device.renderFlags(["mask", "startcap-undefined"], [])
```

### Device colorspaces

### Device.prototype.setDefaultColorSpaces(defaultCS)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Change the set of default device colorspaces to one given.

* **Arguments:**
  * **defaultCS** ([`DefaultColorSpaces`](DefaultColorSpaces.md#DefaultColorSpaces)) – The new set of default colorspaces.

```javascript
var defaultCS = new DefaultColorSpaces()
defaultCS.setDefaultRGB(defaultCS.getDefaultGray())
device.setDefaultColorSpaces(new DefaultColorSpaces())
```

### Layers

### Device.prototype.beginLayer(name)

Begin a marked-content layer with the given name.

* **Arguments:**
  * **name** (`string`) – Name of this marked-content layer.

```javascript
device.beginLayer("my tag")
```

### Device.prototype.endLayer()

End a marked-content layer.

```javascript
device.endLayer()
```

### Structures

### Device.prototype.beginStructure(structure, raw, index)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Begin a [standard structure type](../../common/glossary.md#term-Standard-Structure-Type) with the raw tag name and a unique identifier.

* **Arguments:**
  * **structure** (`string`) – One of the pre-defined structure types in PDF.
  * **raw** (`string`) – The tag name.
  * **index** (`number`) – A unique identifier.

```javascript
device.beginStructure("Document", "my_tag_name", 123)
```

### Device.prototype.endStructure()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

End a standard structure element.

```javascript
device.endStructure()
```

### Metatext

### Device.prototype.beginMetatext(meta, text)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Begin meta text where meta is either of:

- “ActualText”
- “Alt”
- “Abbreviation”
- “Title”

* **Arguments:**
  * **meta** (`string`) – The meta text type
  * **text** (`string`) – The text value.

```javascript
device.beginMetatext("Title", "My title")
```

### Device.prototype.endMetatext()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

End meta text information.

```javascript
device.endMetatext()
```
