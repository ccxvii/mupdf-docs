# mutool sign

The `sign` command reads an input PDF file and by default prints information
about each signature field object. The command applies to the signature field
objects given, or every signature field if none are specified. Given suitable
option signature fields can be verified, cleared or signed using a given
certificate and certificate password.

```bash
mutool sign [options] input.pdf [signature object numbers]
```

`[options]`
: Options are as follows:
  <br/>
  `-p` password
  : Use the specified password if the file is encrypted.
  <br/>
  `-v`
  : Verify each signature field and check whether the document has changed since signing.
  <br/>
  `-c`
  : Revert each signed signature field back to its unsigned state.
  <br/>
  `-s` certificate file
  : Sign each signature field with the certificate in the given file.
  <br/>
  `-P` certificate password
  : The password used together with the certificate to sign a signature field.
  <br/>
  `-o` filename
  : Output PDF file name.

`input.pdf`
: The input PDF document.

`[signature object numbers]`
: Can be used to specify a particular set of signature field objects to apply the `sign` command to.

## Signing

Signing digital signatures in MuPDF requires that you have a PFX certificate.
You can create a self-signed certificate using OpenSSL by following these
steps:

1. Generate a self-signed certificate and private key:

```bash
openssl req -x509 -days 365 -newkey rsa:2048 -keyout cert.pem -out cert.pem
```

1. Convert to PFX format:

```bash
openssl pkcs12 -export -in cert.pem -out cert.pfx
```

## Examples

The following command should show all signature fields in the PDF whether signed or unsigned:

```bash
mutool sign -v unsigned.pdf
```

Once you know the object number of an unsigned signature field, then do the following:

```bash
mutool sign -s certificate.pfx -P password123 -o signed.pdf unsigned.pdf 4242
```

This assumes that object `4242 0 R` is the signature field. Re-running `sign -v` on `signed.pdf` should then show that the signature was signed.

To clear a signature use the `-c` option:

```bash
mutool sign -c -o unsigned.pdf signed.pdf 4242
```
