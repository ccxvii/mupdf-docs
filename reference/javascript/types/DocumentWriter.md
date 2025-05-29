# DocumentWriter

DocumentWriter objects are used to create new documents in several formats.

## Constructors

### *class* DocumentWriter(buffer, format, options)

### *class* DocumentWriter(filename, format, options)

Create a new document writer to create a document with the specified format and output options. If `format` is `null` it is inferred from the filename. The `options` argument is a comma separated list of flags and key-value pairs.

For supported output `format` values and `options` see
[Document Writer Options](../../common/document-writer-options.md).

* **Arguments:**
  * **buffer** ([`Buffer`](Buffer.md#Buffer)) – The buffer to output to.
  * **filename** ([`Buffer`](Buffer.md#Buffer)) – The file name to output to.
  * **format** (`string`) – The file format.
  * **options** (`string`) – The options as key-value pairs.

```javascript
var writer = new mupdf.DocumentWriter(buffer, "PDF", "")
```

## Instance methods

### DocumentWriter.prototype.beginPage(mediabox)

Begin rendering a new page. Returns a [Device](Device.md) that can be used to render the page graphics.

* **Arguments:**
  * **mediaBox** ([`Rect`](Rect.md#Rect)) – The page size.
* **Returns:**
  [Device](Device.md)

```javascript
var device = writer.beginPage([0, 0, 100, 100])
```

### DocumentWriter.prototype.endPage()

Finish the page rendering.

```javascript
writer.endPage()
```

### DocumentWriter.prototype.close()

Finish the document and flush any pending output. It is a
requirement to make this call to ensure that the output file is
complete.

```javascript
writer.close()
```
