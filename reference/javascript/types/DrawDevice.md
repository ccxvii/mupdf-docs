# DrawDevice

The DrawDevice can be used to render to a [Pixmap](Pixmap.md); either by running a Page
using [`Page.prototype.run()`](Page.md#Page.prototype.run) with it or by calling the device methods directly.

## Constructors

### *class* DrawDevice(matrix, pixmap)

Create a device for drawing into a [Pixmap](Pixmap.md). The [Pixmap](Pixmap.md) bounds used should match the transformed page bounds, or you can adjust them to only draw a part of the page.

* **Arguments:**
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – The matrix to apply.
  * **pixmap** ([`Pixmap`](Pixmap.md#Pixmap)) – The pixmap that will be drawn to.

```javascript
var drawDevice = new mupdf.DrawDevice(mupdf.Matrix.identity, pixmap)
```
