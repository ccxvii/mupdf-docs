# mutool poster

The `poster` command reads the input PDF file and for each page chops it up into `x` by `y` pieces. Each piece becomes its own page in the output PDF file. This makes it possible for each page to be printed upscaled and can then be merged into a large poster.

```bash
mutool poster [options] input.pdf [output.pdf]
```

`[options]`
: Options are as follows:
  <br/>
  `-p` password
  : Use the specified password if the file is encrypted.
  <br/>
  `-m` margin
  : Set the margin (overlap) between pages in points or percent.
  <br/>
  `-x` x decimation factor
  : Pieces to horizontally divide each page into.
  <br/>
  `-y` y decimation factor
  : Pieces to vertically divide each page into.
  <br/>
  `-r`
  : Split horizontally from right to left (default splits from left to right).

`input.pdf`
: The input PDF document.

`[output.pdf]`
: The output filename. Defaults to “out.pdf” if not supplied.
