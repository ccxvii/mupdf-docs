# PDFDocument

The PDFDocument is a specialized subclass of [Document](Document.md) which has
additional methods that are only available for PDF files.

## PDF Objects

A PDF document contains objects: dictionaries, arrays, names, strings, numbers,
booleans, and indirect references.
Some dictionaries also have attached data. These are called streams,
and may be compressed.

At the root of the PDF document is the trailer object; which contains pointers to the meta
data dictionary and the catalog object, which in turn contains references to the pages and
forms and everything else.

Pointers in PDF are called indirect references, and are of the form
32 0 R (where 32 is the object number, 0 is the generation, and R is
magic syntax). All functions in MuPDF dereference indirect
references automatically.

> PDFObjects are always bound to the document that created them. Do
> **NOT** mix and match objects from one document with another
> document!

## Constructors

### *class* PDFDocument()

Create a brand new PDF document instance that begins empty with no pages.

To open an existing PDF document, use [`Document.openDocument()`](Document.md#Document.openDocument).

## Static methods

### PDFDocument.formatURIFromPathAndDest(path, destination)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Format a link URI given a
[system independent path](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G8.1640868)
to a remote document and a destination object or a destination
string suitable for [`Page.prototype.createLink()`](Page.md#Page.prototype.createLink).

* **Arguments:**
  * **path** (`string`) – An absolute or relative path to a remote document file.
  * **destination** (`Link | string`) – Link or string referring to a destination using either a destination object or a destination name in the remote document.

### PDFDocument.appendDestToURI(uri, destination)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Append a fragment representing a document destination to a an
existing URI that points to a remote document. The resulting
string is suitable for [`Page.prototype.createLink()`](Page.md#Page.prototype.createLink).

* **Arguments:**
  * **uri** (`string`) – An URI to a remote document file.
  * **destination** (`Link | string | number`) – A Link or string referring to a destination using either a destination object or a destination name in the remote document, or a page number.

## Instance methods

### PDFDocument.prototype.needsPassword()

Returns true if a password is required to open a password protected PDF.

* **Returns:**
  boolean

```javascript
var needsPassword = document.needsPassword()
```

### PDFDocument.prototype.authenticatePassword(password)

Returns a bitfield value against the password authentication result.

The values returned by this interface are interpreted like this:

|   Bit | Description            |
|-------|------------------------|
|     0 | Failed                 |
|     1 | No password needed     |
|     2 | User password is okay  |
|     4 | Owner password is okay |
* **Arguments:**
  * **password** (`string`) – The password to attempt authentication with.
* **Returns:**
  number

```javascript
var auth = document.authenticatePassword("abracadabra")
```

### PDFDocument.prototype.hasPermission(permission)

Returns true if the document has permission for the supplied permission parameter.

These are the recognized permission strings:

| String        | The Document may…                                        |
|---------------|----------------------------------------------------------|
| print         | … be printed.                                            |
| edit          | … be edited.                                             |
| copy          | … be copied.                                             |
| annotate      | … have annotations added/removed.                        |
| form          | … have form field contents edited.                       |
| accessibility | … be copied for accessibility.                           |
| assemble      | … have its pages rearranged.                             |
| print-hq      | … be printed in high quality be printed in high quality. |
* **Arguments:**
  * **permission** (`string`) – The permission to seek for, e.g. “edit”.
* **Returns:**
  boolean

```javascript
var canEdit = document.hasPermission("edit")
```

### PDFDocument.prototype.getVersion()

Returns the PDF document version as an integer multiplied by
10, so e.g. a PDF-1.4 document would return 14.

* **Returns:**
  number

```javascript
var version = pdfDocument.getVersion()
```

### PDFDocument.prototype.setLanguage(lang)

Set the document’s [language code](../../common/glossary.md#term-Language-code).

* **Arguments:**
  * **lang** (`string`)

```javascript
pdfDocument.setLanguage("en")
```

### PDFDocument.prototype.getLanguage()

Get the document’s [language code](../../common/glossary.md#term-Language-code).

* **Returns:**
  string | null

```javascript
var lang = pdfDocument.getLanguage()
```

### PDFDocument.prototype.wasPureXFA()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns whether the document was an XFA form without AcroForm
fields.

* **Returns:**
  boolean

```javascript
var wasPureXFA = pdfDocument.wasPureXFA()
```

### PDFDocument.prototype.wasRepaired()

Returns whether the document was repaired when opened.

* **Returns:**
  boolean

```javascript
var wasRepaired = pdfDocument.wasRepaired()
```

### PDFDocument.prototype.loadNameTree(treeName)

Return an object whose properties and their values come from
corresponding names/values from the given name tree.

* **Returns:**
  Object

```javascript
var dests = pdfDocument.loadNameTree("Dests")
for (var p in dests) {
        console.log("Destination: " + p)
}
```

## Objects

### PDFDocument.prototype.newNull()

Create a new null object.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newNull()
```

### PDFDocument.prototype.newBoolean(v)

Create a new boolean object.

* **Arguments:**
  * **v** (`boolean`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newBoolean(true)
```

### PDFDocument.prototype.newInteger(v)

Create a new integer object.

* **Arguments:**
  * **v** (`number`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newInteger(1)
```

### PDFDocument.prototype.newReal(v)

Create a new real number object.

* **Arguments:**
  * **v** (`number`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newReal(7.3)
```

### PDFDocument.prototype.newString(v)

Create a new string object.

* **Arguments:**
  * **v** (`string`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newString("hello")
```

### PDFDocument.prototype.newByteString(v)

Create a new byte string object.

* **Arguments:**
  * **v** (`Uint8Array | Array of number`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newByteString([21, 31])
```

### PDFDocument.prototype.newName(v)

Create a new name object.

* **Arguments:**
  * **v** (`string`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newName("hello")
```

### PDFDocument.prototype.newIndirect(objectNumber, generation)

Create a new indirect object.

* **Arguments:**
  * **objectNumber** (`number`)
  * **generation** (`number`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newIndirect(42, 0)
```

### PDFDocument.prototype.newArray()

Create a new array object.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newArray()
```

### PDFDocument.prototype.newDictionary()

Create a new dictionary object.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.newDictionary()
```

## Indirect objects

### PDFDocument.prototype.getTrailer()

The trailer dictionary. This contains indirect references to the “Root”
and “Info” dictionaries.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var dict = doc.getTrailer()
```

### PDFDocument.prototype.countObjects()

Return the number of objects in the PDF.

* **Returns:**
  number

```javascript
var num = doc.countObjects()
```

### PDFDocument.prototype.createObject()

Allocate a new numbered object in the PDF, and return an indirect
reference to it. The object itself is uninitialized.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = doc.createObject()
```

### PDFDocument.prototype.deleteObject(num)

Delete the object referred to by an indirect reference or its object number.

* **Arguments:**
  * **num** (`PDFObject | number`) – Delete the referenced object number.

```javascript
doc.deleteObject(obj)
```

### PDFDocument.prototype.addObject(obj)

Add obj to the PDF as a numbered object, and return an indirect reference to it.

* **Arguments:**
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject)) – Object to add.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var ref = doc.addObject(obj)
```

### PDFDocument.prototype.addStream(buf, obj)

Create a stream object with the contents of buffer, add it to the PDF, and return an indirect reference to it. If object is defined, it will be used as the stream object dictionary.

* **Arguments:**
  * **buf** (`Buffer | ArrayBuffer | Uint8Array | string`) – Buffer whose data to put into stream.
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject)) – The object to add the stream to.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var stream = doc.addStream(buffer, object)
```

### PDFDocument.prototype.addRawStream(buf, obj)

Create a stream object with the contents of buffer, add it to the PDF, and return an indirect reference to it. If object is defined, it will be used as the stream object dictionary. The buffer must contain already compressed data that matches “Filter” and “DecodeParms” set in the stream object dictionary.

* **Arguments:**
  * **buf** (`Buffer | ArrayBuffer | Uint8Array | string`) – Buffer whose data to put into stream.
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject)) – The object to add the stream to.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var stream = doc.addRawStream(buffer, object)
```

## Page Tree

### PDFDocument.protoype.findPage(number)

Return the [PDFObject](PDFObject.md) for a page number.

* **Arguments:**
  * **number** (`number`) – The page number, the first page is number zero.
* **Throws:**
  Error on out of range page numbers.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = pdfDocument.findPage(0)
```

### PDFDocument.prototype.findPageNumber(page)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Find a given [PDFPage](PDFPage.md) and return its page number.
If the page can not be found, returns -1.

* **Arguments:**
  * **page** ([`PDFPage`](PDFPage.md#PDFPage))
* **Returns:**
  number

```javascript
var pageNumber = pdfDocument.findPageNumber(page)
```

### PDFDocument.prototype.lookupDest(obj)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Find the destination corresponding to a specific named
destination given as a name or byte string in the form of a
[PDFObject](PDFObject.md).

Returns null if the named destination does not exist.

* **Arguments:**
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject))
* **Returns:**
  [PDFObject](PDFObject.md) | null

```javascript
var destination = pdfDocument.lookupDest(nameobj)
```

### PDFDocument.prototype.rearrangePages(pages)

Rearrange (re-order and/or delete) pages in the PDFDocument.

The pages in the document will be rearranged according to the
input list. Any pages not listed will be removed, and pages may
be duplicated by listing them multiple times.

The PDF objects describing removed pages will remain in the
file and take up space (and can be recovered by forensic tools)
unless you save with the “garbage” option set, see [`PDFDocument.prototype.save()`](#PDFDocument.prototype.save).

* **Arguments:**
  * **pages** (`Array of number`) – An array of page numbers, each page number is 0-based.

```javascript
var document = new Document.openDocument("my_pdf.pdf")
pdfDocument.rearrangePages([3,2])
pdfDocument.save("fewer_pages.pdf", "garbage")
```

### PDFDocument.prototype.insertPage(at, page)

Insert the page’s [PDFObject](PDFObject.md) into the page tree at the page
number specified by `at` (numbered from 0). If `at` is -1,
the page is inserted at the end of the document.

* **Arguments:**
  * **at** (`number`) – The index to insert at.
  * **page** ([`PDFObject`](PDFObject.md#PDFObject)) – The PDFObject representing the page to insert.

```javascript
pdfDocument.insertPage(-1, page)
```

### PDFDocument.prototype.deletePage(index)

Delete the page at the given index.

* **Arguments:**
  * **index** (`number`) – The page number, the first page is number zero.

```javascript
pdfDocument.deletePage(0)
```

### PDFDocument.prototype.addPage(mediabox, rotate, resources, contents)

Create a new [PDFPage](PDFPage.md) object. Note: this function does NOT add
it to the page tree, use [`PDFDocument.prototype.insertPage()`](#PDFDocument.prototype.insertPage)
to do that.

Creation of page contents is described in detail in the PDF
specification’s section on [Content Streams](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G8.1913072).

* **Arguments:**
  * **mediabox** ([`Rect`](Rect.md#Rect)) – Describes the dimensions of the page.
  * **rotate** (`number`) – Rotation value.
  * **resources** ([`PDFObject`](PDFObject.md#PDFObject)) – Resources dictionary object.
  * **contents** (`Buffer | ArrayBuffer | Uint8Array | string`) – Contents string. This represents the page content stream.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var helvetica = pdfDocument.addSimpleFont(new mupdf.Font("Helvetica"), "Latin")
var fonts = pdfDocument.newDictionary()
fonts.put("F1", helvetica)
var resources = pdfDocument.addObject(pdfDocument.newDictionary())
resources.put("Font", fonts)
var pageObject = pdfDocument.addPage(
        [0,0,300,350],
        0,
        resources,
        "BT /F1 12 Tf 100 100 Td (Hello, world!) Tj ET"
)
pdfDocument.insertPage(-1, pageObject)
```

## Resources

### PDFDocument.prototype.addSimpleFont(font, encoding)

Create a [PDFObject](PDFObject.md) from the [Font](Font.md) object as a simple font.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font))
  * **encoding** (`"Latin" | "Greek" | "Cyrillic"`) – Which 8-bit encoding to use. Defaults to “Latin”.

See [`Font.SIMPLE_ENCODING_LATIN`](Font.md#Font.SIMPLE_ENCODING_LATIN), etc.

* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = pdfDocument.addSimpleFont(new mupdf.Font("Times-Roman"), "Latin")
```

### PDFDocument.prototype.addCJKFont(font, language, wmode, style)

Create a [PDFObject](PDFObject.md) from the [Font](Font.md) object as a UTF-16 encoded
CID font for the given language (“zh-Hant”, “zh-Hans”, “ko”, or
“ja”), writing mode (“H” or “V”), and style (“serif” or
“sans-serif”).

* **Arguments:**
  * **font** ([`Font`](Font.md#Font))
  * **language** (`string`)
  * **wmode** (`number`) – `0` for horizontal writing, and `1` for vertical writing.
  * **style** (`string`)
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = pdfDocument.addCJKFont(new mupdf.Font("ja"), "ja", 0, "serif")
```

### PDFDocument.prototype.addFont(font)

Create a [PDFObject](PDFObject.md) from the [Font](Font.md) object as an Identity-H
encoded CID font.

* **Arguments:**
  * **font** ([`Font`](Font.md#Font))
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = pdfDocument.addFont(new mupdf.Font("Times-Roman"))
```

### PDFDocument.prototype.addImage(image)

Create a [PDFObject](PDFObject.md) from the [Image](Image.md) object.

* **Arguments:**
  * **image** ([`Image`](Image.md#Image))
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var obj = pdfDocument.addImage(new mupdf.Image(pixmap))
```

### PDFDocument.prototype.loadImage(obj)

Load an [Image](Image.md) from a [PDFObject](PDFObject.md) (typically an indirect
reference to an image resource).

* **Arguments:**
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject))
* **Returns:**
  [Image](Image.md)

```javascript
var image = pdfDocument.loadImage(obj)
```

## Embedded/Associated files

### PDFDocument.protoype.addEmbeddedFile(filename, mimetype, contents, creationDate, modificationDate, addChecksum)

Embedded a file into the document. If a checksum is added then
the file contents can be verified later. An indirect reference
to a [file specification](../../common/glossary.md#term-File-specification) object is returned.

The returned [file specification](../../common/glossary.md#term-File-specification) object can later be e.g.
connected to an annotation using
[`PDFAnnotation.prototype.setFilespec()`](PDFAnnotation.md#PDFAnnotation.prototype.setFilespec).

* **Arguments:**
  * **filename** (`string`)
  * **mimetype** (`string`) – The [MIME-type](../../common/glossary.md#term-MIME-type).
  * **contents** (`Buffer | ArrayBuffer | Uint8Array | string`)
  * **creationDate** (`Date`)
  * **modificationDate** (`Date`)
  * **addChecksum** (`boolean`) – Defaults to false.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var fileSpecObject = pdfDocument.addEmbeddedFile(
        "my_file.jpg",
         "image/jpeg",
         buffer,
         new Date(),
         new Date(),
         false
)
```

### PDFDocument.prototype.getEmbeddedFiles()

Returns a record of any embedded files on the this PDFDocument.

* **Returns:**
  Record<string, PDFObject>

### PDFDocument.prototype.deleteEmbeddedFile(filename)

Delete an embedded file by filename.

* **Arguments:**
  * **filename** (`string`) – Name of embedded file to delete.

```javascript
doc.deleteEmbeddedFile("test.txt")
```

### PDFDocument.prototype.insertEmbeddedFile(filename, fileSpecObject)

Insert the given file specification as an embedded file using
the given filename.

* **Arguments:**
  * **filename** (`string`) – Name of the file to insert.
  * **fileSpecObject** ([`PDFObject`](PDFObject.md#PDFObject)) – [File specification](../../common/glossary.md#term-File-specification).

```javascript
pdfDocument.insertEmbeddedFile("test.txt", fileSpecObject)
pdfDocument.deleteEmbeddedFile("test.txt")
```

### PDFDocument.prototype.getFilespecParams(fileSpecObject)

Get the file specification parameters from the [file specification](../../common/glossary.md#term-File-specification).

* **Arguments:**
  * **fileSpecObject** ([`PDFObject`](PDFObject.md#PDFObject)) – [file specification](../../common/glossary.md#term-File-specification) object.
* **Returns:**
  [PDFFilespecParams](PDFFilespecParams.md)

```javascript
var obj = pdfDocument.getFilespecParams(fileSpecObject)
```

### PDFDocument.prototype.getEmbeddedFileContents(fileSpecObject)

Returns a [Buffer](Buffer.md) with the contents of the embedded file
referenced by `fileSpecObject`.

* **Arguments:**
  * **fileSpecObject** (`Object`) – [file specification](../../common/glossary.md#term-File-specification)
* **Returns:**
  [Buffer](Buffer.md) | null

```javascript
var buffer = pdfDocument.getEmbeddedFileContents(fileSpecObject)
```

### PDFDocument.prototype.verifyEmbeddedFileChecksum(fileSpecObject)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Verify the MD5 checksum of the embedded file contents.

* **Arguments:**
  * **fileSpecObject** (`Object`) – [file specification](../../common/glossary.md#term-File-specification).
* **Returns:**
  boolean

```javascript
var fileChecksumValid = pdfDocument.verifyEmbeddedFileChecksum(fileSpecObject)
```

### PDFDocument.prototype.isFilespec(object)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Check if the given `object` is a [file specification](../../common/glossary.md#term-File-specification).

* **Arguments:**
  * **object** ([`PDFObject`](PDFObject.md#PDFObject))
* **Returns:**
  boolean

```javascript
var isFilespec = pdfDocument.isFilespec(obj)
```

### PDFDocument.prototype.isEmbeddedFile(object)

Check if the given `object` is a [file specification](../../common/glossary.md#term-File-specification) representing a file
embedded into the PDF document.

* **Arguments:**
  * **object** ([`PDFObject`](PDFObject.md#PDFObject))
* **Returns:**
  boolean

```javascript
var isFilespecObject = pdfDocument.isEmbeddedFile(obj)
```

### PDFDocument.prototype.countAssociatedFiles()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return the number of [associated files](../../common/glossary.md#term-Associated-File)
associated with this document. Note that this is the number of
files associated at the document level, not necessarily the
total number of files associated with elements throughout the
entire document.

* **Returns:**
  number

```javascript
var count = pdfDocument.countAssociatedFiles()
```

### PDFDocument.prototype.associatedFile(n)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return the [file specification](../../common/glossary.md#term-File-specification) object that represents the nth
[associated file](../../common/glossary.md#term-Associated-File) for this document.

`n` should be in the range `0 <= n < countAssociatedFiles()`.

Returns null if no associated file exists or index is out of range.

* **Returns:**
  [PDFObject](PDFObject.md) | null

```javascript
var obj = pdfDocument.associatedFile(0)
```

## Grafting

### PDFDocument.prototype.newGraftMap()

Create a graft map on the destination document, so that objects that have already been copied can be found again. Each graft map should only be used with one source document. Make sure to create a new graft map for each source document used.

* **Returns:**
  [PDFGraftMap](PDFGraftMap.md)

```javascript
var graftMap = doc.newGraftMap()
```

### PDFDocument.prototype.graftObject(obj)

Deep copy an object into the destination document. This function will
not remember previously copied objects. If you are copying several
objects from the same source document using multiple calls, you
should use a graft map instead, see
[`PDFDocument.prototype.newGraftMap()`](#PDFDocument.prototype.newGraftMap).

* **Arguments:**
  * **obj** ([`PDFObject`](PDFObject.md#PDFObject)) – The object to graft.
* **Returns:**
  [PDFObject](PDFObject.md)

```javascript
var copiedObj = doc.graftObject(obj)
```

### PDFDocument.prototype.graftPage(to, srcDoc, srcPage)

Graft a page and its resources at the given page number from the source document to the requested page number in the document.

* **Arguments:**
  * **to** (`number`) – The page number to insert the page before. Page numbers start at 0 and -1 means at the end of the document.
  * **srcDoc** ([`PDFDocument`](#PDFDocument)) – Source document.
  * **srcPage** (`number`) – Source page number.

This would copy the first page of the source document (0) to the last page (-1) of the current PDF document.

```javascript
doc.graftPage(-1, srcDoc, 0)
```

## Journalling

### PDFDocument.prototype.enableJournal()

Activate journalling for the document.

```javascript
pdfDocument.enableJournal()
```

### PDFDocument.prototype.getJournal()

Returns a Javascript object with a property indicating the
current position, and the names of each entry in the
undo/redo journal history:

`{ position: number, steps: Array of string }`

* **Returns:**
  Object

```javascript
var journal = pdfDocument.getJournal()
```

### PDFDocument.prototype.beginOperation(op)

Begin a journal operation. Each call to begin an operation
should be paired with a call to either
[`PDFDocument.prototype.endOperation()`](#PDFDocument.prototype.endOperation) if the operation was
successful, or to [`PDFDocument.prototype.abandonOperation()`](#PDFDocument.prototype.abandonOperation) if
it failed.

* **Arguments:**
  * **op** (`string`) – The name of the operation.

```javascript
pdfDocument.beginOperation("Change annotation color")
// Change the annotation
pdfDocument.endOperation()
```

### PDFDocument.prototype.beginImplicitOperation()

Begin an implicit journal operation. Implicit operations are
operations that happen due to other operations, and that should
not be subdivided into separate undo steps. E.g. editing several
attributes of a [PDFAnnotation](PDFAnnotation.md) might be desirable to do in a
single undo step. See [`PDFDocument.prototype.beginOperation()`](#PDFDocument.prototype.beginOperation)
for the requirements about paired calls.

```javascript
pdfDocument.beginOperation("Complex operation")
pdfDocument.beginImplicitOperation()
pdfDocument.endOperation()
pdfDocument.beginImplicitOperation()
pdfDocument.endOperation()
pdfDocument.endOperation()
```

### PDFDocument.prototype.endOperation()

End a previously started normal or implicit operation. After
this it can be [`undone`](#PDFDocument.prototype.undo) and
[`redone`](#PDFDocument.prototype.redo).

```javascript
pdfDocument.endOperation()
```

### PDFDocument.prototype.abandonOperation()

Abandon a normal or implicit operation. Reverts to the state
before that operation began. This is normally called if an
operation failed for some reason.

```javascript
pdfDocument.abandonOperation()
```

### PDFDocument.prototype.canUndo()

Returns whether undo is possible in this state.

* **Returns:**
  boolean

```javascript
var canUndo = pdfDocument.canUndo()
```

### PDFDocument.prototype.canRedo()

Returns whether redo is possible in this state.

* **Returns:**
  boolean

```javascript
var canRedo = pdfDocument.canRedo()
```

### PDFDocument.prototype.undo()

Move backwards in the undo history. Changes to the document
after this call will throw away all subsequent undo history.

```javascript
pdfDocument.undo()
```

### PDFDocument.prototype.redo()

Move forwards in the undo history.

```javascript
pdfDocument.redo()
```

### PDFDocument.prototype.saveJournal(filename)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Save the undo/redo journal to a file.

* **Arguments:**
  * **filename** (`string`) – File to save the journal to.

```javascript
pdfDocument.saveJournal("test.journal")
```

## Layers

### PDFDocument.prototype.countLayers()

Return the number of optional content layers in this document.

* **Returns:**
  number

```javascript
var count = pdfDocument.countLayers()
```

### PDFDocument.prototype.isLayerVisible(n)

Return whether layer `n` is visible, where `n` should be in
the interval `0 <= n < countLayers()`.

* **Arguments:**
  * **n** (`number`) – What layer to check visibility of.
* **Returns:**
  boolean

```javascript
var visible = pdfDocument.isLayerVisible(1)
```

### PDFDocument.prototype.setLayerVisible(n, visible)

Set layer `n` to be visible or invisible, where `n` is
in the interval `0 <= n < countLayers()`.

Pages affected by a visibility change, need to be processed
again for the layers to be visible/invisible.

* **Arguments:**
  * **n** (`number`) – What layer to change visibility for.
  * **visible** (`boolean`) – Whether the layer should be visible.
* **Returns:**
  number

```javascript
pdfDocument.setLayerVisible(1, true)
```

### getLayerName(n)

Return the name of layer number `n`, where `n` is
`0 <= n < countLayers()`.

* **Returns:**
  string

```javascript
var name = pdfDocument.getLayerName(0)
```

## Page Labels

### PDFDocument.prototype.setPageLabels(index, style, prefix, start)

Sets the page label numbering for the page and all pages following it, until the next page with an attached label.

* **Arguments:**
  * **index** (`number`) – The start page index to start labeling from.
  * **style** (`string`) – Can be one of the following strings: “” (none), “D” (decimal), “R” (roman numerals upper-case), “r” (roman numerals lower-case), “A” (alpha upper-case), or “a” (alpha lower-case).
  * **prefix** (`string`) – Define a prefix for the labels.
  * **start** (`number`) – The ordinal with which to start numbering.

```javascript
doc.setPageLabels(0, "D", "Prefix", 1)
```

### PDFDocument.prototype.deletePageLabels(index)

Removes any associated page label from the page.

* **Arguments:**
  * **index** (`number`)

```javascript
doc.deletePageLabels(0)
```

## Saving

### PDFDocument.prototype.canBeSavedIncrementally()

Returns whether the document can be saved incrementally, e.g.
repaired documents or applying redactions prevents incremental
saves.

* **Returns:**
  boolean

```javascript
var canBeSavedIncrementally = pdfDocument.canBeSavedIncrementally()
```

### PDFDocument.prototype.saveToBuffer(options)

Saves the document to a Buffer.

* **Arguments:**
  * **options** (`string`) – See [PDF Write Options](../../common/pdf-write-options.md).
* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = doc.saveToBuffer("garbage=2,compress=yes")
```

### PDFDocument.prototype.save(filename, options)

Saves the document to a file.

* **Arguments:**
  * **filename** (`string`)
  * **options** (`string`) – See [PDF Write Options](../../common/pdf-write-options.md)

```javascript
doc.save("out.pdf", "incremental")
```

### PDFDocument.prototype.countVersions()

Returns the number of versions of the document in a PDF file,
typically 1 + the number of updates.

* **Returns:**
  number

```javascript
var versionNum = pdfDocument.countVersions()
```

### PDFDocument.prototype.hasUnsavedChanges()

Returns true if the document has been changed since it was last
opened or saved.

* **Returns:**
  boolean

```javascript
var hasUnsavedChanges = pdfDocument.hasUnsavedChanges()
```

### PDFDocument.prototype.countUnsavedVersions()

Returns the number of unsaved updates to the document.

* **Returns:**
  number

```javascript
var unsavedVersionNum = pdfDocument.countUnsavedVersions()
```

### PDFDocument.prototype.validateChangeHistory()

Check the history of the document, and determine the last
version that checks out OK. Returns `0` if the entire history
is OK, `1` if the next to last version is OK, but the last
version has issues, etc.

* **Returns:**
  number

```javascript
var changeHistory = pdfDocument.validateChangeHistory()
```

## Processing

### PDFDocument.prototype.subsetFonts()

Scan the document and establish which glyphs are used from each
font, next rewrite the font files such that they only contain
the used glyphs. By removing unused glyphs the size of the font
files inside the PDF will be reduced.

```javascript
pdfDocument.subsetFonts()
```

### PDFDocument.prototype.bake(bakeAnnots, bakeWidgets)

*Baking* a document changes all the annotations and/or form fields (otherwise known as widgets) in the document into static content. It “bakes” the appearance of the annotations and fields onto the page, before removing the interactive objects so they can no longer be changed.

Effectively this removes the “annotation or “widget” type of these objects, but keeps the appearance of the objects.

* **Arguments:**
  * **bakeAnnots** (`boolean`) – Whether to bake annotations or not. Defaults to true.
  * **bakeWidgets** (`boolean`) – Whether to bake widgets or not. Defaults to true.

## AcroForm Javascript

### PDFDocument.prototype.enableJS()

Enable interpretation of document Javascript actions.

```javascript
pdfDocument.enableJS()
```

### PDFDocument.prototype.disableJS()

Disable interpretation of document Javascript actions.

```javascript
pdfDocument.disableJS()
```

### PDFDocument.prototype.isJSSupported()

Returns whether interpretation of document Javascript actions
is supported. Interpretation of Javascript may be disabled at
build time.

* **Returns:**
  boolean

```javascript
var jsIsSupported = pdfDocument.isJSSupported()
```

### PDFDocument.prototype.setJSEventListener(listener)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Calls the listener whenever a document Javascript action
triggers an event.

At present the only callback the listener will be used for
is an alert event.

* **Arguments:**
  * **listener** (`Object`) – The Javascript listener function.

```javascript
pdfDocument.setJSEventListener({
                onAlert: function(message) {
                                print(message)
                }
})
```

## ZUGFeRD

### PDFDocument.prototype.zugferdProfile()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Determine if the current PDF is a ZUGFeRD PDF, and, if so,
return the profile type in use. Possible return values include:
“NOT ZUGFERD”, “COMFORT”, “BASIC”, “EXTENDED”, “BASIC WL”,
“MINIMUM”, “XRECHNUNG”, and “UNKNOWN”.

* **Returns:**
  string

```javascript
var profile = pdfDocument.zugferdProfile()
```

### PDFDocument.prototype.zugferdVersion()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Determine if the current PDF is a ZUGFeRD PDF, and, if so,
return the version of the spec it claims to conform to.
Returns 0 for non-zugferd PDFs.

* **Returns:**
  number

```javascript
var version = pdfDocument.zugferdVersion()
```

### PDFDocument.prototype.zugferdXML()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return a buffer containing the embedded ZUGFeRD XML data from
this ZUGFeRD PDF.

* **Returns:**
  [Buffer](Buffer.md) | null

```javascript
var buf = pdfDocument.zugferdXML()
```
