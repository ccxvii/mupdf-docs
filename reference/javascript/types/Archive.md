# Archive

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

## Constructors

### *class* Archive(path)

Create a new archive based either on a tar- or zip-file or the contents of a directory.

* **Arguments:**
  * **path** (`string`) – Path string to the archive file or directory.

```javascript
var archive1 = new mupdf.Archive("example1.zip")
var archive2 = new mupdf.Archive("example2.tar")
var archive3 = new mupdf.Archive("images/")
```

## Instance methods

### Archive.prototype.getFormat()

Returns a string describing the archive format.

* **Returns:**
  string

```javascript
var archive = new mupdf.Archive("example1.zip")
print(archive.getFormat())
```

### Archive.prototype.countEntries()

Returns the number of entries in the archive.

* **Returns:**
  number

```javascript
var archive = new mupdf.Archive("example1.zip")
var numEntries = archive.countEntries()
```

### Archive.prototype.listEntry(idx)

Returns the name of entry number idx in the archive, or null if idx is
out of range.

* **Arguments:**
  * **idx** (`number`) – Entry index.
* **Returns:**
  string | null

```javascript
var archive = new mupdf.Archive("example1.zip")
var entry = archive.listEntry(0)
```

### Archive.prototype.hasEntry(name)

Returns true if an entry of the given name exists in the archive.

* **Arguments:**
  * **name** (`string`) – Entry name to look for.
* **Returns:**
  boolean

```javascript
var archive = new mupdf.Archive("example1.zip")
var hasEntry = archive.hasEntry("file1.txt")
```

### Archive.prototype.readEntry(name)

Returns the contents of the entry of the given name.

* **Arguments:**
  * **name** (`string`) – Name of entry to look for.
* **Returns:**
  [Buffer](Buffer.md)

```javascript
var archive = new mupdf.Archive("example1.zip")
var contents = archive.readEntry("file1.txt")
```

## Examples

```javascript
var archive = new mupdf.Archive("example1.zip")
var n = archive.countEntries()
for (var i = 0; i < n; ++i) {
        var entry = archive.listEntry(i)
        var contents = archive.readEntry(entry)
        console.log("entry", entry, contents.length)
}
```
