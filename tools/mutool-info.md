# mutool info

The `info` command shows info about the objects on pages in an input file. For each page the page object number is shown, and then detailed information about the desired objects.

```bash
mutool info [options] file [pages]
```

`[options]`
: Options are as follows:
  <br/>
  `-p` password
  : Use the specified password if the file is encrypted.
  <br/>
  `-F`
  : List fonts.
  <br/>
  `-I`
  : List images.
  <br/>
  `-M`
  : List dimensions.
  <br/>
  `-P`
  : List patterns.
  <br/>
  `-S`
  : List shadings.
  <br/>
  `-X`
  : List form and postscript xobjects.
  <br/>
  `-Z`
  : List ZUGFeRD info.

`file`
: The input file.

`[pages]`
: Comma separated list of page numbers and ranges. If no pages are
  supplied then all document pages will be considered for the output
  file.
