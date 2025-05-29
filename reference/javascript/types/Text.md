# Text

A Text object contains text. See the [`Device.prototype.fillText`](Device.md#Device.prototype.fillText).

## Constructors

### *class* Text()

Create a new empty text object.

```javascript
var text = new mupdf.Text()
```

## Instance methods

### Text.prototype.getBounds(strokeState, transform)

Get the bounds of the instance.

* **Arguments:**
  * **strokeState** ([`StrokeState`](StrokeState.md#StrokeState)) – The stroking state.
  * **transform** ([`Matrix`](Matrix.md#Matrix)) – The transformation matrix.

### Text.prototype.showGlyph(font, trm, gid, uni, wmode)

Add a glyph to the text object.

Transform is the text matrix, specifying font size and glyph location.
For example: `[size, 0, 0, -size, x, y]`.

Glyph and unicode may be `-1` for n-to-m cluster mappings. For
example, the “fi” ligature would be added in two steps: first the glyph
for the ‘fi’ ligature and the unicode value for ‘f’; then glyph `-1`
and the unicode value for ‘i’.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font)) – Font object.
  * **trm** ([`Matrix`](Matrix.md#Matrix)) – Transformation matrix.
  * **gid** (`number`) – Glyph id.
  * **uni** (`number`) – Unicode codepoint.
  * **wmode** (`number`) – `0` (default) for horizontal writing, and `1` for vertical writing.

```javascript
text.showGlyph(new mupdf.Font("Times-Roman"), mupdf.Matrix.identity, 21, 0x66, 0)
text.showGlyph(new mupdf.Font("Times-Roman"), mupdf.Matrix.identity, -1, 0x69, 0)
```

### Text.prototype.showString(font, trm, str, wmode)

Add a simple string to this Text object. Will do font substitution if the font does not have all the unicode characters required.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font)) – Font object.
  * **trm** ([`Matrix`](Matrix.md#Matrix)) – Transformation matrix.
  * **str** (`string`) – Content for Text object.
  * **wmode** (`number`) – `0` (default) for horizontal writing, and `1` for vertical writing.

```javascript
text.showString(new mupdf.Font("Times-Roman"), mupdf.Matrix.identity, "Hello World")
```

### Text.prototype.walk(walker)

* **Arguments:**
  * **walker** ([`TextWalker`](TextWalker.md#TextWalker)) – Function with protocol methods, see example below for details.

```javascript
function print(...args) {
        console.log(args.join(" "))
}

var textPrinter = {
        beginSpan: function (f, m, wmode, bidi, dir, lang) {
                print("beginSpan", f, m, wmode, bidi, dir, Q(lang))
        },
        showGlyph: function (f, m, g, u, v, b) { print("glyph", f, m, g, String.fromCodePoint(u), v, b) },
        endSpan: function () { print("endSpan") }
}

var traceDevice = {
        fillText: function (text, ctm, colorSpace, color, alpha) {
                print("fillText", ctm, colorSpace, color, alpha)
                text.walk(textPrinter)
        },
        clipText: function (text, ctm) {
                print("clipText", ctm)
                text.walk(textPrinter)
        },
        strokeText: function (text, stroke, ctm, colorSpace, color, alpha) {
                print("strokeText", Q(stroke), ctm, colorSpace, color, alpha)
                text.walk(textPrinter)
        },
        clipStrokeText: function (text, stroke, ctm) {
                print("clipStrokeText", Q(stroke), ctm)
                text.walk(textPrinter)
        },
        ignoreText: function (text, ctm) {
                print("ignoreText", ctm)
                text.walk(textPrinter)
        }
}

var doc = mupdf.PDFDocument.openDocument(fs.readFileSync("test.pdf"), "application/pdf")
var page = doc.loadPage(0)
var device = new mupdf.Device(traceDevice)
page.run(device, mupdf.Matrix.identity)
```
