# Functions

Most functionality is provided by member functions of class objects.
There are just a couple of top level configuration functions listed
here.

### mupdf.installLoadFontFunction(callback)

Install a handler to load system (or missing) fonts.

The callback function will be called with four arguments:

```javascript
callback(fontName, scriptName, isBold, isItalic)
```

The callback should return either a [`Font`](types/Font.md#Font) object for the requested
font, or `null` if an exact match cannot be found (so that the font
loading machinery can keep looking through the chain of fallback
fonts).

### mupdf.enableICC()

Enable ICC-profiles based operation.

### mupdf.disableICC()

Disable ICC-profiles based operation.

### mupdf.setUserCSS(stylesheet, useDocumentStyles)

Set a style sheet to apply to all reflowable documents.

* **Arguments:**
  * **stylesheet** (`string`) – The CSS text to use.
  * **useDocumentStyles** (`boolean`) – Whether to respect the document’s own style sheet.
