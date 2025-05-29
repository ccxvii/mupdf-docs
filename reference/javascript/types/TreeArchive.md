# TreeArchive

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

## Constructors

### *class* TreeArchive()

Create a new empty tree archive.

```javascript
var treeArchive = new mupdf.TreeArchive()
```

## Instance methods

### TreeArchive.prototype.add(name, buffer)

Add a named buffer to a tree archive.

* **Arguments:**
  * **name** (`string`) – Name of archive entry to add.
  * **buffer** (`Buffer | ArrayBuffer | Uint8Array | string`) – Buffer of data to store with the entry.

```javascript
var buf = new mupdf.Buffer()
buf.writeLine("hello world!")
var archive = new mupdf.TreeArchive()
archive.add("file2.txt", buf)
print(archive.hasEntry("file2.txt"))
```
