# PathWalker

## Constructors

### *class* PathWalker()

*This is an interface, not a concrete class!*

An object implementing this interface of optional callback functions
can be used to get calls whenever [`Path.prototype.walk()`](Path.md#Path.prototype.walk) iterates over a
basic drawing operation corresponding to that of the function name.

### closePath()

Called when [`Path.prototype.walk()`](Path.md#Path.prototype.walk) encounters a close subpath operation.

### curveTo(x1, y1, x2, y2, x3, y3)

Called when [`Path.prototype.walk()`](Path.md#Path.prototype.walk) encounters an operation drawing a Bézier
curve from the current point to (x3, y3) using (x1, y1) and (x2, y2)
as control points.

![image](images/curveTo.svg)
* **Arguments:**
  * **x1** (`number`) – X1 coordinate.
  * **y1** (`number`) – Y1 coordinate.
  * **x2** (`number`) – X2 coordinate.
  * **y2** (`number`) – Y2 coordinate.
  * **x3** (`number`) – X3 coordinate.
  * **y3** (`number`) – Y3 coordinate.

### lineTo(x, y)

Called when [`Path.prototype.walk()`](Path.md#Path.prototype.walk) encounters an operation drawing a straight
line from the current point to the given point.

![image](images/lineTo.svg)
* **Arguments:**
  * **x** (`number`) – X coordinate.
  * **y** (`number`) – Y coordinate.

### moveTo(x, y)

Called when [`Path.prototype.walk()`](Path.md#Path.prototype.walk) encounters an operation moving the pen to
the given point, beginning a new subpath and sets the current point.

* **Arguments:**
  * **x** (`number`) – X coordinate.
  * **y** (`number`) – Y coordinate.
