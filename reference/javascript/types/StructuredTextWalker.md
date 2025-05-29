# StructuredTextWalker

A structured text walker is an object with (optional) callback methods
used to iterate over the contents of a [StructuredText](StructuredText.md).

## Constructors

### *class* StructuredTextWalker()

*This is an interface, not a concrete class!*

On beginLine the direction parameter is a vector (e.g. [0, 1]) and
can you can calculate the rotation as an angle with some trigonometry on the vector.

### beginTextBlock(bbox)

Called before every text block in the [StructuredText](StructuredText.md).

* **Arguments:**
  * **bbox** ([`Rect`](Rect.md#Rect))

### endTextBlock()

Called after every text block.

### beginLine(bbox, wmode, direction)

Called before every line of text in a block.

* **Arguments:**
  * **bbox** ([`Rect`](Rect.md#Rect))
  * **wmode** (`number`)
  * **direction** ([`Point`](Point.md#Point))

### endLine()

Called after every line of text.

### beginStruct(standard, raw, index)

Called to indicate that a new structure element begins. May not
be neatly nested within blocks or lines.

* **Arguments:**
  * **standard** (`string`)
  * **raw** (`string`)
  * **index** (`number`)

### endStruct()

Called after every structure element.

### onChar(c, origin, font, size, quad, color)

Called for every character in a line of text.

* **Arguments:**
  * **c** (`string`)
  * **origin** ([`Point`](Point.md#Point))
  * **font** ([`Font`](Font.md#Font))
  * **size** (`number`)
  * **quad** ([`Quad`](Quad.md#Quad))
  * **color** (`Color`)

### onImageBlock(bbox, transform, image)

Called for every image in a [StructuredText](StructuredText.md) if its options were
set to preserve images.

* **Arguments:**
  * **bbox** ([`Rect`](Rect.md#Rect))
  * **transform** ([`Matrix`](Matrix.md#Matrix))
  * **image** ([`Image`](Image.md#Image))

### onVector(flags, rgb)

Called for every vector in a [StructuredText](StructuredText.md) if its options
were set to collect vectors.

* **Arguments:**
  * **flags** (`Object`)
  * **rgb** (`Array of number`)
