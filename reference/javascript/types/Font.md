# Font

Font objects can be created from TrueType, OpenType,
Type1 or CFF fonts. In PDF there are also special
Type3 fonts.

## Constructors

### *class* Font(name, data, index)

### *class* Font(name, filename, index)

Create a new font. Either from a buffer, a file name, or a
built-in font name.

The built-in standard PDF fonts are:

- `"Times-Roman"`
- `"Times-Italic"`
- `"Times-Bold"`
- `"Times-BoldItalic"`
- `"Helvetica"`
- `"Helvetica-Oblique"`
- `"Helvetica-Bold"`
- `"Helvetica-BoldOblique"`
- `"Courier"`
- `"Courier-Oblique"`
- `"Courier-Bold"`
- `"Courier-BoldOblique"`
- `"Symbol"`
- `"ZapfDingbats"`

The built-in CJK fonts are referenced by language code:
`"zh-Hant"`, `"zh-Hans"`, `"ja"`, `"ko"`.

* **Arguments:**
  * **name** (`string`) – Font name.
  * **data** (`Buffer | ArrayBuffer | Uint8Array`) – The font data if loaded from a buffer.
  * **filename** (`string`) – The name of the font file to load.
  * **index** (`number`) – Subfont index (only used for TTC fonts).

```javascript
var font1 = new mupdf.Font("Times-Roman")
var font2 = new mupdf.Font("ko")
var font3 = new mupdf.Font("Comic Sans", "/usr/share/fonts/truetype/msttcorefonts/Comic_Sans_MS.ttf")
var font4 = new mupdf.Font("Comic Sans", "/usr/share/fonts/truetype/msttcorefonts/Comic_Sans_MS.ttf", 1)
var font5 = new mupdf.Font("Freight Sans", fs.readFileSync("DroidSansFallbackFull.ttf"))
```

## Constants

Encoding constants for [`PDFDocument.prototype.addSimpleFont`](PDFDocument.md#PDFDocument.prototype.addSimpleFont):

### Font.SIMPLE_ENCODING_LATIN

WinAnsiEncoding (a.k.a. CP-1252)

### Font.SIMPLE_ENCODING_GREEK

ISO-8859-7

### Font.SIMPLE_ENCODING_CYRILLIC

KOI8-U

## Instance methods

### Font.prototype.getName()

Get the font name.

* **Returns:**
  string

```javascript
var name = font.getName()
```

### Font.prototype.encodeCharacter(unicode)

Get the glyph index for a unicode character. Glyph `0` is returned if the font does not have a glyph for the character.

* **Arguments:**
  * **unicode** (`number | string`) – The unicode character, or the first unicode character of a string.
* **Returns:**
  number.

```javascript
var index = font.encodeCharacter(0x42)
```

### Font.prototype.advanceGlyph(glyph, wmode)

Return advance width for a glyph in either horizontal or vertical writing mode.

* **Arguments:**
  * **glyph** (`number`) – The glyph as unicode character.
  * **wmode** (`number`) – `0` for horizontal writing, and `1` for vertical writing, defaults to horizontal.
* **Returns:**
  number

```javascript
var width = font.advanceGlyph(0x42)
```

### Font.prototype.isBold()

Returns `true` if font is bold.

* **Returns:**
  boolean

```javascript
var isBold = font.isBold()
```

### Font.prototype.isItalic()

Returns `true` if font is italic.

* **Returns:**
  boolean

```javascript
var isItalic = font.isItalic()
```

### Font.prototype.isMono()

Returns `true` if font is monospaced.

* **Returns:**
  boolean

```javascript
var isMono = font.isMono()
```

### Font.prototype.isSerif()

Returns `true` if font is serif.

* **Returns:**
  boolean

```javascript
var isSerif = font.isSerif()
```
