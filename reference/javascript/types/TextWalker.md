# TextWalker

### *class* TextWalker()

An object implementing this interface of optional callback functions
can be used to get calls whenever [`Text.prototype.walk()`](Text.md#Text.prototype.walk) iterates over a text object.

### beginSpan(font, trm, wmode, bidiLevel, markupDirection, language)

Called before every text span in the [Text](Text.md) being walked.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font))
  * **trm** ([`Matrix`](Matrix.md#Matrix))
  * **wmode** (`number`)
  * **bidiLevel** (`int`)
  * **markupDirection** (`int`)
  * **language** (`string`)

### endSpan()

Called at the end of every span in the text.

### showGlyph(font, trm, gid, ucs, wmode, bidiLevel)

Called once per character for in a text span.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font))
  * **trm** ([`Matrix`](Matrix.md#Matrix))
  * **gid** (`number`)
  * **ucs** (`number`)
  * **wmode** (`number`)
  * **bidiLevel** (`number`)
