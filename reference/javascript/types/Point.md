# Point

A Point describes a point in space. It is represented by an array with two coordinates.

## Constructors

### *class* Point()

*This is an interface, not a concrete class!*

Points are not represented by a class; they are just plain arrays of two numbers.

## Static methods

### Point.transform(point, matrix)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Transforms the supplied point by the given transformation matrix.

* **Arguments:**
  * **point** ([`Point`](#Point)) – Point to transform.
  * **matrix** ([`Matrix`](Matrix.md#Matrix)) – Matrix describing transformation to perform.
* **Returns:**
  [Point]()

```javascript
var m = mupdf.Point.transform([100, 100], [1, 0.5, 1, 1, 1, 1])
```
