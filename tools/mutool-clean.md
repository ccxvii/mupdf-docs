# mutool clean

The `clean` command pretty prints and rewrites the syntax of a PDF file. It can be used to repair broken files, expand compressed streams, filter out a range of pages, etc.

```bash
mutool clean [options] input.pdf [output.pdf] [pages]
```

`[options]`
: Options are as follows:
  <br/>
  `-p` password
  : Use the specified password if the file is encrypted.
  <br/>
  `-g`
  : Garbage collect unused objects.
  <br/>
  `-gg`
  : In addition to `-g` compact xref table.
  <br/>
  `-ggg`
  : In addition to `-gg` merge duplicate objects.
  <br/>
  `-gggg`
  : In addition to `-ggg` check streams for duplication.
  <br/>
  `-l`
  : Linearize PDF (no longer supported!).
  <br/>
  `-D`
  : Save file without encryption.
  <br/>
  `-E` encryption
  : Save file with new encryption (`rc4-40`, `rc4-128`, `aes-128`, or `aes-256`).
  <br/>
  `-O` owner_password
  : Owner password (only if encrypting).
  <br/>
  `-U` user_password
  : User password (only if encrypting).
  <br/>
  `-P` permission
  : Permission flags (only if encrypting).
  <br/>
  `-a`
  : ASCII hex encode binary streams.
  <br/>
  `-d`
  : Decompress streams.
  <br/>
  `-z`
  : Deflate uncompressed streams.
  <br/>
  `-f`
  : Compress font streams.
  <br/>
  `-i`
  : Compress image streams.
  <br/>
  `-c`
  : Pretty-print graphics commands in content streams.
  <br/>
  `-s`
  : Sanitize graphics commands in content streams.
  <br/>
  `-t`
  : Compact object syntax.
  <br/>
  `-tt`
  : Use indented object syntax to make PDF objects more readable.
  <br/>
  `-L`
  : Print comments containing labels showing how each object can be reached from the Root.
  <br/>
  `-A`
  : Create appearance streams for annotations that are missing appearance streams.
  <br/>
  `-AA`
  : Recreate appearance streams for all annotations.
  <br/>
  `-m`
  : Preserve metadata.
  <br/>
  `-S`
  : Subset fonts if possible. (EXPERIMENTAL!)
  <br/>
  `-Z`
  : Use object streams cross reference streams for extra compression.
  <br/>
  `--(color|gray|bitonal)-(|lossy-|lossless-)image-subsample-method method`
  : Set the subsampling method (`average`, or `bicubic`) for the desired image types, for example color-lossy and bitonal-lossless.
  <br/>
  `--(color|gray|bitonal)-(|lossy-|lossless-)image-subsample-dpi dpi`
  : Set the resolution at which to subsample.
  <br/>
  `--(color|gray|bitonal)-(|lossy-|lossless-)image-recompress-method quality`
  : Set the recompression quality to either of `never`, `same`, `lossless`, `jpeg`, `j2k`, `fax`, or `jbig2`.
  <br/>
  `--structure=keep|drop`
  : Keep or drop the structure tree.

`input.pdf`
: Input file name. Must be a PDF file.

`[output.pdf]`
: The output file. Must be a PDF file.
  <br/>
  If no output file is specified, it will write the cleaned PDF to “out.pdf” in the current directory.

`[pages]`
: Comma separated list of page numbers and ranges (for example:
  1,5,10-15,20-N), where the character N denotes the last page. If no
  pages are specified, then all pages will be included.
