# mutool recolor

The `recolor` command changes the colorspaces used in
a PDF file.

```bash
mutool recolor [options] file
```

`[options]`
: Options are as follows:
  <br/>
  `-c` colorspace
  : The desired colorspace used in the output document: `gray` (default), `rgb` or `cmyk`.
  <br/>
  `-o` output file
  : Use the specified filename for the output PDF file with altered colorspaces.
  <br/>
  `-r`
  : Remove any output intents present in the input file.

`file`
: Input file name. The input must be a PDF file.
