# PDFObject

All functions that take a [PDFObject]() apply an automatic translation between
Javascript objects and [PDFObject]() using a few basic rules:

- `null`, `true`, `false`, and numbers are translated directly.
- Strings are translated to PDF names, unless they are surrounded by
  parentheses: `"Foo"` becomes the `/Foo` and `"(Foo)"` becomes
  `(Foo)`.
- Arrays and dictionaries are recursively translated to PDF arrays and dictionaries.
  Be aware of cycles though! The translation does NOT cope with cyclic references!

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>
This automatic translation goes both ways – entries of PDF dictionaries and
arrays can be accessed like Javascript objects and arrays.

## Constructors

### *class* PDFObject()

*You cannot create instances of this class with the new operator!*

Use the methods on a [PDFDocument](PDFDocument.md) instance to create new objects.

## Instance properties

### PDFObject.prototype.length

Number of entries in array PDFObjects. Zero for all other object types.

### PDFObject.prototype.[n]

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Get or set the element at index `n` in an array (0-indexed).

See [`PDFObject.prototype.get()`](#PDFObject.prototype.get) and [`PDFObject.prototype.put()`](#PDFObject.prototype.put) for the equivalent in mupdf.js.

* **Throws:**
  Error on out of bounds accesses.

```javascript
var pdfObject = pdfDocument.newArray()
pdfObject[0] = "hello"
pdfObject[1] = "world"
```

### PDFObject.prototype.[name]

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Access a key named `name` in a dictionary. It is both possible to
get and set its value, but also delete the key entirely.

See [`PDFObject.prototype.get()`](#PDFObject.prototype.get), [`PDFObject.prototype.put()`](#PDFObject.prototype.put), and
[`PDFObject.prototype.delete()`](#PDFObject.prototype.delete) for the equivalent in mupdf.js.

```javascript
var pages = doc.getTrailer().Root.Pages
pages.Hello = "world"
pages["Xyzzy"] = 42
delete pages["Hello"]
delete pages.Xyzzy
```

## Instance methods

### PDFObject.prototype.get(...path)

Access dictionaries and arrays in the [PDFObject]().

Returns null if the dictionary key does not exist, or if
the array index is out of range.

* **Arguments:**
  * **...path** (`Array`) – The path.
* **Returns:**
  [PDFObject]() | null

```javascript
var dict = pdfDocument.newDictionary()
var value = dict.get("my_key")
var arr = pdfDocument.newArray()
var value = arr.get(1)
var page7 = pdfDocument.getTrailer().get("Root", "Pages", "Kids", 7)
```

### PDFObject.prototype.getInheritable(key)

For a dictionary, if the requested key does not exist,
getInheritable() will walk Parent references to parent
dictionaries and lookup the same key there.

If no key can be found in any parent or grand-parent or
grand-grand-parent, all the way up, `null` is returned.

* **Arguments:**
  * **key** (`PDFObject | string`)
* **Returns:**
  [PDFObject]() | null

```javascript
var page = pdfDocument.loadPage(0)
var pageObj = page.getObject()
var rotate = pageObj.getInheritable("Rotate")
```

### PDFObject.prototype.put(key, value)

Set values in [PDFObject]() dictionaries or arrays.

* **Arguments:**
  * **key** (`PDFObject | string | number`) – Interpreted as an index for arrays or a key string for dictionaries.
  * **value** (`PDFObject | Array | string | number | boolean | null`) – The value to set at the array index or for dictionary key.

```javascript
var dict = pdfDocument.newDictionary()
dict.put("my_key", "my_value")
var arr = pdfDocument.newArray()
arr.put(0, 42)
```

### PDFObject.prototype.delete(key)

Delete a reference from a [PDFObject]().

* **Arguments:**
  * **key** (`number | string | PDFObject`)

```javascript
var dict = pdfDocument.newDictionary()
dict.put("my_key", "my_value")
dict.delete("my_key")
var arr = pdfDocument.newArray()
arr.put(1, 42)
arr.delete(1)
```

### PDFObject.prototype.resolve()

If the object is an indirect reference, return the object it points to; otherwise return the object itself.

* **Returns:**
  [PDFObject]()

```javascript
var resolvedObj = obj.resolve()
```

### PDFObject.prototype.isArray()

* **Returns:**
  boolean

```javascript
var result = obj.isArray()
```

### PDFObject.prototype.isDictionary()

* **Returns:**
  boolean

```javascript
var result = obj.isDictionary()
```

### PDFObject.prototype.forEach(callback)

Iterate over all the entries in a dictionary or array and call a function for each value-key pair.

* **Arguments:**
  * **callback** – `(val: PDFObject, key: number | string, self: PDFObject) => void`

```javascript
obj.forEach(function (value, key) {
        console.log("value="+value+",key="+key)
})
```

### PDFObject.prototype.push(item)

Append item to the end of the object.

* **Arguments:**
  * **item** ([`PDFObject`](#PDFObject))

```javascript
obj.push("item")
```

### PDFObject.prototype.toString(tight, ascii)

Returns the object as a pretty-printed string.

* **Arguments:**
  * **tight** (`boolean`) – Whether to print the object as tightly as possible, or as human-readably as possible.
  * **ascii** (`boolean`) – Whether to print binary data as ascii or as binary data.
* **Returns:**
  string

```javascript
var str = obj.toString()
```

### PDFObject.prototype.valueOf()

Try to convert a PDF object into a corresponding primitive Javascript value.

Indirect references are converted to the string “obj 0 R” where obj
is the PDF object’s object number.

Names are converted to strings.

Arrays and dictionaries are not converted.

* **Returns:**
  A Javascript value or this.

```javascript
var val = obj.valueOf()
```

### PDFObject.prototype.compare(other_obj)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Compare the object to another one. Returns 0 on match, non-zero
on mismatch.

* **Arguments:**
  * **other** ([`PDFObject`](#PDFObject))
* **Returns:**
  number

```javascript
var match = pdfObj.compare(other_obj)
```

## Streams

The only way to access a stream is via an indirect object, since all streams are numbered objects.

### PDFObject.prototype.isStream()

Returns whether the object is an indirect reference pointing to a stream.

* **Returns:**
  boolean

```javascript
var val = obj.isStream()
```

### PDFObject.prototype.readStream()

Read the contents of the stream object into a [Buffer](Buffer.md).

* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = obj.readStream()
```

### PDFObject.prototype.readRawStream()

Read the raw, uncompressed, contents of the stream object into a
[Buffer](Buffer.md).

* **Returns:**
  [Buffer](Buffer.md)

```javascript
var buffer = obj.readRawStream()
```

### PDFObject.prototype.writeObject(obj)

Update the object the indirect reference points to.

* **Arguments:**
  * **obj** ([`PDFObject`](#PDFObject))

```javascript
obj.writeObject(obj)
```

### PDFObject.prototype.writeStream(buf)

Update the contents of the stream the indirect reference points to.
This will update the “Length”, “Filter” and “DecodeParms” automatically.

* **Arguments:**
  * **buf** (`Buffer | ArrayBuffer | Uint8Array`)

```javascript
obj.writeStream(buffer)
```

### PDFObject.prototype.writeRawStream(buf)

Update the contents of the stream the indirect reference points to.
The buffer must contain already compressed data that matches
the “Filter” and “DecodeParms”. This will update the “Length”
automatically, but leave the “Filter” and “DecodeParms” untouched.

* **Arguments:**
  * **buf** (`Buffer | ArrayBuffer | Uint8Array`)

```javascript
obj.writeRawStream(buffer)
```

## Primitive Objects

Primitive PDF objects such as booleans, names, and numbers can usually be
treated like Javascript values (thanks to valueOf). When that is not sufficient
use these functions:

### PDFObject.prototype.isNull()

Returns true if the object is null.

* **Returns:**
  boolean

```javascript
var val = obj.isNull()
```

### PDFObject.prototype.isBoolean()

Returns whether the object is a boolean.

* **Returns:**
  boolean

```javascript
var val = obj.isBoolean()
```

### PDFObject.prototype.asBoolean()

Get the boolean primitive value.

* **Returns:**
  boolean

```javascript
var val = obj.asBoolean()
```

### PDFObject.prototype.isInteger()

Returns whether the object is an integer.

* **Returns:**
  boolean

```javascript
var val = obj.isInteger()
```

### PDFObject.prototype.isReal()

Returns whether the object is a PDF real number.

* **Returns:**
  boolean

```javascript
var val = pdfObj.isReal()
```

### PDFObject.prototype.isNumber()

Returns whether the object is a number (an integer or a real).

* **Returns:**
  boolean

```javascript
var val = obj.isNumber()
```

### PDFObject.prototype.asNumber()

Get the number primitive value.

* **Returns:**
  number

```javascript
var val = obj.asNumber()
```

### PDFObject.prototype.isName()

Returns whether the object is a name.

* **Returns:**
  boolean

```javascript
var val = obj.isName()
```

### PDFObject.prototype.asName()

Get the name as a string.

* **Returns:**
  string

```javascript
var val = obj.asName()
```

### PDFObject.prototype.isString()

Returns whether the object is a string.

* **Returns:**
  boolean

```javascript
var val = obj.isString()
```

### PDFObject.prototype.asString()

Convert a “text string” to a Javascript unicode string.

* **Returns:**
  string

```javascript
var val = obj.asString()
```

### PDFObject.prototype.asByteString()

Convert a string to an array of byte values.

* **Returns:**
  Uint8Array | Array of number

```javascript
var val = obj.asByteString()
```

### PDFObject.prototype.isIndirect()

Is the object an indirect reference.

* **Returns:**
  boolean

```javascript
var val = obj.isIndirect()
```

### PDFObject.prototype.asIndirect()

Return the object number the indirect reference points to.

* **Returns:**
  number

```javascript
var val = obj.asIndirect()
```
