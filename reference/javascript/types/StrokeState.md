# StrokeState

A StrokeState controls the properties of how stroking operations are performed.
Besides controlling the line width, it is also possible to control
[line cap style](../../common/glossary.md#term-Line-Cap-Style), [line join style](../../common/glossary.md#term-Line-Join-Style), and the [miter limit](../../common/glossary.md#term-Miter-Limit).

## Constructors

### *class* StrokeState(template)

Create a new empty stroke state object.

The Javascript object used as template allows for setting
the following attributes:

* “lineCap”: string: The [line cap style](../../common/glossary.md#term-Line-Cap-Style) to be used. One of `"Butt" | "Round" | "Square"`
* “lineJoin”: string: The [line join style](../../common/glossary.md#term-Line-Join-Style) to be used. One of `"Miter" | "Round" | "Bevel"`
* “lineWidth”: number: The line width to be used.
* “miterLimit”: number: The [miter limit](../../common/glossary.md#term-Miter-Limit) to be used.
* “dashPhase”: number: The dash phase to be used.
* “dashPattern”: Array of number: The sequence of dash lengths to be used.

* **Arguments:**
  * **template** (`Object`) – An object with the parameters to set.

```javascript
var strokeState = new mupdf.StrokeState({
        lineCap: "Square",
        lineJoin: "Bevel",
        lineWidth: 2.0,
        miterLimit: 1.414,
        dashPhase: 11,
        dashPattern: [ 2, 3 ]
})
```

## Instance methods

### StrokeState.prototype.getLineCap()

Get the [line cap style](../../common/glossary.md#term-Line-Cap-Style).

* **Returns:**
  `"Butt" | "Round" | "Square"`

```javascript
var lineCap = strokeState.getLineCap()
```

### StrokeState.prototype.getLineJoin()

Get the [line join style](../../common/glossary.md#term-Line-Join-Style).

* **Returns:**
  `"Miter" | "Round" | "Bevel"`

```javascript
var lineJoin = strokeState.getLineJoin()
```

### StrokeState.prototype.getLineWidth()

Get the line line width.

* **Returns:**
  number

```javascript
var width = strokeState.getLineWidth()
```

### StrokeState.prototype.getMiterLimit()

Get the [miter limit](../../common/glossary.md#term-Miter-Limit).

* **Returns:**
  number

```javascript
var limit = strokeState.getMiterLimit()
```

### StrokeState.prototype.getDashPhase()

Get the dash pattern phase (where in the dash pattern stroking starts).

* **Returns:**
  number

```javascript
var limit = strokeState.getDashPhase()
```

### StrokeState.prototype.getDashPattern()

Get the dash pattern as an array of numbers specifying alternating
lengths of dashes and gaps, or null if none set.

* **Returns:**
  Array of number | null

```javascript
var dashPattern = strokeState.getDashPattern()
```
