# MultiArchive

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

## Constructors

### *class* MultiArchive()

Create a new empty multi archive.

```javascript
var multiArchive = new mupdf.MultiArchive()
```

## Instance methods

### MultiArchive.prototype.mountArchive(subArchive, path)

Add an archive to the set of archives handled by a multi archive.
If `path` is `null`, the `subArchive` contents appear at the
top-level, otherwise they will appear prefixed by the string
`path`.

* **Arguments:**
  * **subArchive** ([`Archive`](Archive.md#Archive)) – An archive that will be a child archive of this one.
  * **path** (`string`) – The path at which the archive will be inserted.

In the following example `example1.zip` contains `file1.txt` and
`example2.zip` contains `file2.txt`. The MultiArchive now lets you
access both `file1.txt` and `subpath/file2.txt`:

```javascript
var archive = new mupdf.MultiArchive()
archive.mountArchive(new mupdf.Archive("example1.zip"), null)
archive.mountArchive(new mupdf.Archive("example2.zip"), "subpath")
console.log(archive.hasEntry("file1.txt"))
console.log(archive.hasEntry("subpath/file2.txt"))
```
