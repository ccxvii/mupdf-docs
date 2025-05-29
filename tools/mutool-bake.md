# mutool bake

The `bake` command changes the colorspaces used in a PDF file.

```bash
mutool bake [options] input.pdf [output.pdf]
```

`[options]`
: Options are as follows:
  <br/>
  `-A`
  : Do not bake annotations into page contents.
  <br/>
  `-F`
  : Do not bake form fields into page contents.
  <br/>
  `-O` options
  : See [PDF Write Options](../reference/common/pdf-write-options.md).
    If none are given `garbage` is used by default.

`input.pdf`
: Name of input PDF file.

`output.pdf`
: Name of output PDF file. If none is given `out.pdf` is used by default.
