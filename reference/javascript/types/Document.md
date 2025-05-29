# Document

MuPDF can open many document types such as PDF, XPS, CBZ, EPUB, FictionBook 2
and a handful of image formats.

### *class* Document()

*You cannot create instances of this class with the new operator!*

## Constants

Permission flags for [`Document.prototype.hasPermission`](#Document.prototype.hasPermission):

### Document.PERMISSION_PRINT

`"print"` – Print the document.

### Document.PERMISSION_EDIT

`"edit"` – Modify the contents of the document by operations other than those controlled by the other flags: (annotate, form, assemble).

### Document.PERMISSION_COPY

`"edit"` – Copy or otherwise extract text from the document.

### Document.PERMISSION_ANNOTATE

`"annotate"` – Add or modify annotations.

### Document.PERMISSION_FORM

`"form"` – Fill in existing form fields.

### Document.PERMISSION_ACCESSIBILITY

`"accessibility"` –
Copy or otherwise extract text from the document in support of accessibility.

### Document.PERMISSION_ASSEMBLE

`"assemble"` – Insert, rotate, or delete pages and create bookmarks.

### Document.PERMISSION_PRINT_HQ

`"print-hq"` – Print the document to a representation from which a faithful digital
copy of the PDF content could be generated.

## Static methods

### Document.openDocument(filename)

### Document.openDocument(buffer, magic)

Open the named or given document.

* **Arguments:**
  * **filename** (`string`) – File name to open.
  * **buffer** (`Buffer | ArrayBuffer | Uint8Array | string`) – Buffer containing a PDF file.
  * **magic** (`string`) – An optional [MIME-type](../../common/glossary.md#term-MIME-type) or file extension. Defaults to “application/pdf”.
* **Returns:**
  Document

```javascript
var document = new mupdf.Document.openDocument("my_pdf.pdf", "application/pdf")
```

## Instance methods

### Document.prototype.needsPassword()

Returns `true` if a password is required to open a password protected PDF.

* **Returns:**
  boolean

```javascript
var needsPassword = document.needsPassword()
```

### Document.prototype.authenticatePassword(password)

Returns a bitfield value against the password authentication result.

* **Arguments:**
  * **password** (`string`) – The password to attempt authentication with.
* **Returns:**
  number

|   **Bitfield value** | **Description**                           |
|----------------------|-------------------------------------------|
|                    0 | Failed                                    |
|                    1 | No password needed                        |
|                    2 | Is User password and is okay              |
|                    4 | Is Owner password and is okay             |
|                    6 | Is both User & Owner password and is okay |
```javascript
var auth = document.authenticatePassword("abracadabra")
```

### Document.prototype.hasPermission(permission)

Check if a user is allowed permission to perform certain operations on the document.

* **Arguments:**
  * **permission** (`"print" | "edit" | "copy" | "annotate" | "form" | "accessibility" | "assemble" | "print-hq"`)

See [`Document.PERMISSION_PRINT`](#Document.PERMISSION_PRINT), etc.

* **Returns:**
  boolean

```javascript
var canEdit1 = document.hasPermission("edit")
var canEdit2 = document.hasPermission(Document.PERMISSION_EDIT)
```

### Document.prototype.getMetaData(key)

Return various meta data information. The common keys are: format, encryption, [info:ModDate](info:ModDate), and [info:Title](info:Title). Returns `undefined` if the meta data does not exist.

* **Arguments:**
  * **key** (`string`) – What metadata type to return.
* **Returns:**
  string | null

```javascript
var format = document.getMetaData("format")
var modificationDate = doc.getMetaData("info:ModDate")
var author = doc.getMetaData("info:Author")
```

### Document.prototype.setMetaData(key, value)

Set document meta data information field to a new value.

* **Arguments:**
  * **key** (`string`) – Metadata key to set.
  * **value** (`string`) – New value to set for the given key.

```javascript
document.setMetaData("info:Author", "My Name")
```

### Document.prototype.isReflowable()

Returns true if the document is reflowable, such as EPUB, FB2 or XHTML.

* **Returns:**
  boolean

```javascript
var isReflowable = document.isReflowable()
```

### Document.prototype.layout(pageWidth, pageHeight, fontSize)

Layout a reflowable document (EPUB, FictionBook2, HTML or XHTML) to fit
the specified page and font sizes.

* **Arguments:**
  * **pageWidth** (`number`) – Desired page width.
  * **pageHeight** (`number`) – Desired page height.
  * **fontSize** (`number`) – Desire font size.

```javascript
document.layout(300, 300, 16)
```

### Document.prototype.countPages()

Count the number of pages in the document. This may change if you call
the layout function with different parameters.

* **Returns:**
  number

```javascript
var numPages = document.countPages()
```

### Document.prototype.loadPage(number)

Returns a [Page](Page.md) object for the given page number.

For documents where [`Document.prototype.isPDF()`](#Document.prototype.isPDF) returns true,
the returned [Page](Page.md) is of the subclass [PDFPage](PDFPage.md).

* **Arguments:**
  * **number** (`number`) – Number of page to load, 0 means the first page in the document.
* **Returns:**
  [Page](Page.md) | [PDFPage](PDFPage.md).

```javascript
var page = document.loadPage(0) // loads the 1st page of the document
```

### Document.prototype.loadOutline()

Returns an array with the outline (also known as table of contents or
bookmarks). In the array is an object for each heading with the
property ‘title’, and a property ‘page’ containing the page number. If
the object has a ‘down’ property, it contains an array with all the
sub-headings for that entry.

* **Returns:**
  Array of [OutlineItem](OutlineItem.md) (nested).

```javascript
var outline = document.loadOutline()
```

### Document.prototype.outlineIterator()

Returns an [OutlineIterator](OutlineIterator.md) for the document outline.

* **Returns:**
  [OutlineIterator](OutlineIterator.md)

```javascript
var obj = document.outlineIterator()
```

### Document.prototype.resolveLink(link)

Resolve a document internal link URI to a page index.

* **Arguments:**
  * **link** (`Link | string`) – A link or a link URI string to resolve.
* **Returns:**
  number

```javascript
var pageNumber = document.resolveLink(my_link)
```

### Document.prototype.resolveLinkDestination(link)

Resolve a document internal link URI to a link destination.

* **Arguments:**
  * **link** (`Link | string`) – A link or a link URI string to resolve.
* **Returns:**
  [LinkDestination](LinkDestination.md)

```javascript
var linkDestination = document.resolveLinkDestination(linkuri)
```

### Document.prototype.isPDF()

Returns `true` if the document is a [PDFDocument](PDFDocument.md).

* **Returns:**
  boolean

```javascript
var isPDF = document.isPDF()
```

### Document.prototype.asPDF()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a PDF version of the document (if possible).
PDF documents return themselves.
Documents that have an underlying PDF representation return that.
Other document types return null.

* **Returns:**
  [PDFDocument](PDFDocument.md) | null

```javascript
var doc = mupdf.Document.openDocument(filename)
var pdf = doc.asPDF()
if (pdf) {
        // the document has a native PDF representation
} else {
        // it does not have a native PDF representation
}
```

### Document.prototype.formatLinkURI(linkDestination)

Format a document internal link destination object to a URI string suitable for [`Page.prototype.createLink()`](Page.md#Page.prototype.createLink).

* **Arguments:**
  * **linkDestination** ([`LinkDestination`](LinkDestination.md#LinkDestination)) – The link destination object to format.
* **Returns:**
  string

```javascript
var uri = document.formatLinkURI({
        chapter: 0,
        page: 42,
        type: "FitV",
        x: 0,
        y: 0,
        width: 100,
        height: 50,
        zoom: 1
})
page.createLink([0, 0, 100, 100], uri)
```
