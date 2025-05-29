# Quad

An object representing a quadrilateral or [QuadPoint](../../common/glossary.md#term-QuadPoint).

In TypeScript the Quad type is defined as follows:

```javascript
type Quad = [ number, number, number, number number, number, number, number ]
```

## Constructors

### *class* Quad()

*This is an interface, not a concrete class!*

Quads are not represented by a class; they are just plain arrays of eight numbers.

## Static properties

### Quad.empty

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

A quad whose coordinates are such that it is categorized as empty.

### Quad.invalid

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

A quad whose coordinates are such that it is categorized as invalid.

### Quad.infinite

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

A quad whose coordinates are such that it is categorized as infinite.

## Static methods

### Quad.isEmpty(quad)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a boolean indicating if the quad is empty or not.

* **Arguments:**
  * **quad** ([`Quad`](#Quad)) – Rectangle to evaluate.
* **Returns:**
  boolean

```javascript
var isEmpty = mupdf.Quad.isEmpty([0, 0, 0, 0]) // true
var isEmpty = mupdf.Quad.isEmpty([0, 0, 100, 100]) // false
```

### Quad.isValid(quad)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a boolean indicating if the quad is valid or not.

* **Arguments:**
  * **quad** ([`Quad`](#Quad)) – Rectangle to evaluate.
* **Returns:**
  boolean

```javascript
var isValid = mupdf.Quad.isValid([0, 0, 100, 100]) // true
var isValid = mupdf.Quad.isValid([0, 0, -100, 100]) // false
```

### Quad.isInfinite(quad)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a boolean indicating if the quad is infinite or not.

* **Arguments:**
  * **quad** ([`Quad`](#Quad)) – Rectangle to evaluate.
* **Returns:**
  boolean

```javascript
var isInfinite = mupdf.Quad.isInfinite([0x80000000, 0x80000000, 0x7fffff80, 0x7fffff80]) //true
var isInfinite = mupdf.Quad.isInfinite([0, 0, 100, 100]) // false
```

### Quad.transform(quad, matrix)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Transforms the supplied quad by the given transformation matrix.

Transforming an invalid, empty or infinite quad results in the
supplied quad being returned without change.

* **Arguments:**
  * **quad** ([`Quad`](#Quad)) – Quad to transform.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – Matrix describing transformation to perform.
* **Returns:**
  [Quad]()

```javascript
var m = mupdf.Quad.transform([0, 0, 100, 100], [1, 0.5, 1, 1, 1, 1])
```

### Quad.isPointInside(quad, point)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return whether the point is inside the quad.

:returns boolean

```javascript
var inside = mupdf.Rect.isPointInside([0, 0, 100, 100], [50, 50])
```

### Quad.quadFromRect(rect)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Create a Quad that maps exactly to the coordinate of rectangle.

* **Arguments:**
  * **rect** ([`Rect`](Rect.md#Rect))
* **Returns:**
  [Quad]()
