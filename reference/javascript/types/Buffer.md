# Buffer

Buffer objects are used for working with binary data, e.g. returned from
`asJPEG`. They can be used much like arrays, but are much more
efficient since they only store bytes.

## Constructors

### *class* Buffer(data)

Creates a new Buffer initialized with contents from data.

* **Arguments:**
  * **data** (`Buffer | ArrayBuffer | Uint8Array | string | null`) – Contents used to populate buffer, or leave it empty if `null`.

```javascript
var buffer = new mupdf.Buffer("hello world")
```

## Instance properties

### Buffer.prototype.length

The number of bytes in this buffer (read-only).

* **Throws:**
  TypeError on attempted writes.
* **Returns:**
  number

### Buffer.prototype.[n]

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Get or set the byte at index `n`.

See [`readByte()`](#Buffer.prototype.readByte) and [`writeByte()`](#Buffer.prototype.writeByte) for the equivalent in mupdf.js.

* **Throws:**
  RangeError on out of bounds accesses.

```javascript
var byte = buffer[0]
```

## Instance methods

### Buffer.prototype.asString()

Returns the contents of this buffer as a string.

* **Returns:**
  string

```javascript
var str = buffer.asString()
```

### Buffer.prototype.asUint8Array()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Returns the contents of this buffer as a Uint8Array.

* **Returns:**
  Uint8Array

```javascript
var arr = buffer.asUint8Array()
```

### Buffer.prototype.getLength()

Returns the number of bytes in this buffer.

* **Returns:**
  number

```javascript
var length = buffer.getLength()
```

### Buffer.prototype.readByte(at)

Returns the byte at the specified index `at` if within `0 <= at < getLength()`. Otherwise returns `undefined`.

* **Arguments:**
  * **at** (`number`) – Index to read byte at.
* **Returns:**
  number

```javascript
buffer.readByte(0)
```

### Buffer.prototype.slice(start, end)

Create a new buffer containing a (subset of) the data in this buffer.
Start and end are offsets from the beginning of this buffer, and if negative from the end of this buffer.
If `start` points to the end of this buffer, or if `end` point to at or before `start`, then an empty buffer will be returned.

* **Arguments:**
  * **start** (`number`) – Start index.
  * **end** (`number`) – End index (optional).
* **Returns:**
  [Buffer]()

```javascript
var buffer = new mupdf.Buffer()
buffer.write("hello world") // buffer contains "hello world"
var newBuffer = buffer.slice(1, -1) // newBuffer contains "ello worl"
```

### Buffer.prototype.write(str)

Append the string as UTF-8 to the end of this buffer.

* **Arguments:**
  * **str** (`string`) – String to append.

```javascript
buffer.write("hello world")
```

### Buffer.prototype.writeBuffer(data)

Append the contents of the `data` buffer to the end of this buffer.

* **Arguments:**
  * **data** (`Buffer | ArrayBuffer | Uint8Array | string`) – Data buffer to append.

```javascript
buffer.writeBuffer(anotherBuffer)
```

### Buffer.prototype.writeByte(byte)

Append a single byte to the end of this buffer.
Only the least significant 8 bits of the value are appended.

* **Arguments:**
  * **byte** (`number`) – The byte value to append.

```javascript
buffer.writeByte(0x2a)
```

### Buffer.prototype.writeLine(str)

Append string to the end of this buffer ending with a newline.

* **Arguments:**
  * **str** (`string`) – String to append.

```javascript
buffer.writeLine("a line")
```

### Buffer.prototype.writeRune(c)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Encode a unicode character as UTF-8 and append to the end of
the buffer.

* **Arguments:**
  * **c** (`number`) – The character unicode codepoint.

```javascript
buffer.writeRune(0x4f60) // To append U+4f60, 你
buffer.writeRune(0x597d) // To append U+597d, 好
buffer.writeRune(0xff01) // To append U+ff01, ！
```

### Buffer.prototype.save(filename)

Write the contents of the buffer to a file.

* **Arguments:**
  * **filename** (`string`) – Filename to save to.

```javascript
buffer.save("buffer.dat")
```
