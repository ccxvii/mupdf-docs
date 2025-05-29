# PDFGraftMap

The graft map is a structure used to copy objects between different PDF documents,
and track which objects have already been copied so that they can be re-used.

## Constructors

### *class* PDFGraftMap()

*You cannot create instances of this class with the new operator!*

Call [`PDFDocument.prototype.newGraftMap`](PDFDocument.md#PDFDocument.prototype.newGraftMap) to create a graft map.

## Instance methods

### PDFGraftMap.prototype.graftObject(obj)

Return a deep copy of the given object suitable for use within
the graft map’s target document.

* **Arguments:**
  * **object** ([`PDFObject`](PDFObject.md#PDFObject)) – The object to graft.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var map = document.newGraftMap()
map.graftObject(obj)
```

### PDFGraftMap.prototype.graftPage(to, srcDoc, srcPage)

Graft a page and its resources at the given page number from
the source document to the requested page number in the
destination document connected to the map.

Page numbers start at 0 and -1 means at the end of the
document.

* **Arguments:**
  * **to** (`number`) – The page number to insert the page before.
  * **srcDoc** ([`PDFDocument`](PDFDocument.md#PDFDocument)) – Source document.
  * **srcPage** (`number`) – Source page number.

```javascript
var map = dstdoc.newGraftMap()
map.graftObject(-1, srcdoc, 0)
```
