# mutool extract

The `extract` command can be used to extract images and font files from a PDF file. The image and font files will be saved out to the same folder which the file originates from.

```bash
mutool extract [options] input.pdf [object numbers]
```

`[options]`
: Options are as follows:
  <br/>
  `-p` password
  : Use the specified password if the file is encrypted.
  <br/>
  `-r`
  : Convert images to RGB when extracting them.
  <br/>
  `-a`
  : Embed SMasks as alpha channel.
  <br/>
  `-N`
  : Do not use ICC color conversions.

`input.pdf`
: Input file name. Must be a PDF file.

`[object numbers]`
: An array of object ids to extract from. If no object numbers are given
  on the command line, all images and fonts will be extracted from the
  file.
