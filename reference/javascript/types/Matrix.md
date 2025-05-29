# Matrix

A Matrix is used to compute transformations of points and graphics
objects.

3-by-3 matrices of this form can perform 2-dimensional transformations
such as translations, rotations, scaling, and skewing:

```default
/ a b 0 \
| c d 0 |
\ e f 1 /
```

Because of the fixes values in the right-most column, such a matrix is
represented in Javascript as an array of six numbers:

```javascript
[a, b, c, d, e, f]
```

In TypeScript the Matrix type is defined as follows:

```javascript
type Matrix = [number, number, number, number, number, number]
```

## Constructors

### *class* Matrix()

*This is an interface, not a concrete class!*

Matrices are not represented by a class; they are just plain arrays of six numbers.

## Static properties

### Matrix.identity

The identity matrix, short hand for `[1, 0, 0, 1, 0, 0]`.

```javascript
var m = mupdf.Matrix.identity
```

## Static methods

### Matrix.scale(sx, sy)

Returns a scaling matrix, short hand for `[sx, 0, 0, sy, 0, 0]`.

* **Arguments:**
  * **sx** (`number`) – X scale as a floating point number.
  * **sy** (`number`) – Y scale as a floating point number.
* **Returns:**
  [Matrix]()

```javascript
var m = mupdf.Matrix.scale(2, 2)
```

### Matrix.translate(tx, ty)

Return a translation matrix, short hand for `[1, 0, 0, 1, tx, ty]`.

* **Arguments:**
  * **tx** (`number`) – X translation as a floating point number.
  * **ty** (`number`) – Y translation as a floating point number.
* **Returns:**
  [Matrix]()

```javascript
var m = mupdf.Matrix.translate(2, 2)
```

### Matrix.rotate(theta)

Return a rotation matrix, short hand for
`[cos(theta), sin(theta), -sin(theta), cos(theta), 0, 0]`.

* **Arguments:**
  * **theta** (`number`) – Rotation in degrees, positive for CW and negative for CCW.
* **Returns:**
  [Matrix]()

```javascript
var m = mupdf.Matrix.rotate(90)
```

### Matrix.concat(a, b)

Concatenate matrices `a` and `b`. Bear in mind that matrix
multiplication is not commutative.

* **Arguments:**
  * **a** ([`Matrix`](#Matrix)) – Left side matrix.
  * **b** ([`Matrix`](#Matrix)) – Right side matrix.
* **Returns:**
  [Matrix]()

```javascript
var m = mupdf.Matrix.concat([1, 1, 1, 1, 1, 1], [2, 2, 2, 2, 2, 2])
// expected result [4, 4, 4, 4, 6, 6]
```

### Matrix.invert(matrix)

Inverts the supplied matrix and returns the result.

* **Arguments:**
  * **matrix** ([`Matrix`](#Matrix)) – Matrix to invert.
* **Returns:**
  [Matrix]()

```javascript
var m = mupdf.Matrix.invert([1, 0.5, 1, 1, 1, 1])
```
