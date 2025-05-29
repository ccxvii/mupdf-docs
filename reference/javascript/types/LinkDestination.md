# LinkDestination

A link destination points to a location within a document and how a document viewer should show that destination.

### *class* LinkDestination(chapter, page, type, x, y, width, height)

* **Arguments:**
  * **chapter** (`number`)
  * **page** (`number`)
  * **type** (`"Fit" | "FitB" | "FitH" | "FitBH" | "FitV" | "FitBV" | "FitR" | "XYZ"`)

## Constants

The possible type values:

### LinkDestination.FIT

Display the page with contents zoomed to make the entire page visible.

### LinkDestination.FIT_H

Scroll to the top coordinate and zoom to make the page width visible.

### LinkDestination.FIT_V

Scroll to the left coordinate and zoom to make the page height visible.

### LinkDestination.FIT_B

Zoom to fit the page bounding box.

### LinkDestination.FIT_BH

Zoom to fit the page bounding box width.

### LinkDestination.FIT_BV

Zoom to fit the page bounding box height.

### LinkDestination.FIT_R

Scroll and zoom to make the specified rectangle visible.

### LinkDestination.XYZ

Display with coordinates at the top left zoomed in to the specified magnification factor.

## Instance properties

### LinkDestination.prototype.chapter

The chapter within the document.

### LinkDestination.prototype.page

The page within the document.

### LinkDestination.prototype.type

Either “Fit”, “FitB”, “FitH”, “FitBH”, “FitV”, “FitBV”, “FitR” or “XYZ”.

The type controls which of the x, y, width, height, and zoom values are used.

### LinkDestination.prototype.x

The left coordinate. Used for “FitV”, “FitBV”, “FitR”, and “XYZ”.

### LinkDestination.prototype.y

The top coordinate. Used for “FitH”, “FitBH”, “FitR”, and “XYZ”.

### LinkDestination.prototype.width

The width of the zoomed in region. Used for “XYZ”.

### LinkDestination.prototype.height

The height of the zoomed in region. Used for “XYZ”.

### LinkDestination.prototype.zoom

The zoom factor. Used for “XYZ”.
