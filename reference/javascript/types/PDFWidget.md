# PDFWidget

[Widgets](../../common/glossary.md#term-Widget-Type) are annotations that represent form
components such as buttons, text inputs and signature fields.

Because PDFWidget inherits [PDFAnnotation](PDFAnnotation.md), they also provide the
same interface as other annotation types.

Many widgets, e.g. text inputs or checkboxes, are the visual representation of
an associated form field. As the widget changes state, so does its
corresponding field value; for example when the text is edited in a text input
or a checkbox is checked. Note that widgets may be changed by Javascript event
handlers triggered by edits on other widgets.

The PDF specification has sections on [Widget annotations](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1951506)
and
[Interactive forms](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1951635)
with further details.

## Constructors

### *class* PDFWidget()

*You cannot create instances of this class with the new operator!*

To get the widgets on a page, see [`PDFPage.prototype.getWidgets()`](PDFPage.md#PDFPage.prototype.getWidgets).

## Instance methods

### PDFWidget.prototype.getFieldType()

Return the widget type, one of the following:

`"button" | "checkbox" | "combobox" | "listbox" | "radiobutton" | "signature" | "text"`

* **Returns:**
  string

```javascript
var type = widget.getFieldType()
```

### PDFWidget.prototype.getFieldFlags()

Return the field flags which indicate attributes for the
field. There are convenience functions to check some of these:
[`isReadOnly`](#PDFWidget.prototype.isReadOnly),
[`isMultiline`](#PDFWidget.prototype.isMultiline),
[`isPassword`](#PDFWidget.prototype.isPassword),
[`isComb`](#PDFWidget.prototype.isComb),
[`isButton`](#PDFWidget.prototype.isButton),
[`isPushButton`](#PDFWidget.prototype.isPushButton),
[`isCheckbox`](#PDFWidget.prototype.isCheckbox),
[`isRadioButton`](#PDFWidget.prototype.isRadioButton),
[`isText`](#PDFWidget.prototype.isText),
[`isChoice`](#PDFWidget.prototype.isChoice),
[`isListBox`](#PDFWidget.prototype.isListBox),
and
[`isComboBox`](#PDFWidget.prototype.isComboBox).

For details refer to the PDF specification’s sections
on flags
[common to all field types](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1697681),
[for button fields](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1697832),
[for text fields](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1967484),
and
[for choice fields](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.7old.pdf#G13.1873701).

* **Returns:**
  number

```javascript
var flags = widget.getFieldFlags()
```

### PDFWidget.prototype.getName()

Retrieve the name of the field.

* **Returns:**
  string

```javascript
var fieldName = widget.getName()
```

### PDFWidget.prototype.getMaxLen()

Get maximum allowed length of the string value.

* **Returns:**
  number

```javascript
var length = widget.getMaxLen()
```

### PDFWidget.prototype.getOptions()

Returns an array of strings which represent the value for each corresponding radio button or checkbox field.

* **Returns:**
  Array of string

```javascript
var options = widget.getOptions()
```

### PDFWidget.prototype.getLabel()

Get the field name as a string.

* **Returns:**
  string

```javascript
var label = widget.getLabel()
```

## Editing

### PDFWidget.prototype.getValue()

Get the widget value.

* **Returns:**
  string

```javascript
var value = widget.getValue()
```

### PDFWidget.prototype.setTextValue(value)

Set the widget string value.

* **Arguments:**
  * **value** (`string`) – New text value.

```javascript
widget.setTextValue("Hello World!")
```

### PDFWidget.prototype.setChoiceValue(value)

Sets the choice value against the widget.

* **Arguments:**
  * **value** (`string`) – New choice value.

```javascript
widget.setChoiceValue("Yes")
```

### PDFWidget.prototype.toggle()

Toggle the state of the widget, returns true if the state changed.

* **Returns:**
  boolean

```javascript
var state = widget.toggle()
```

### PDFWidget.prototype.getEditingState()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Get whether the widget is in editing state.

* **Returns:**
  boolean

```javascript
var state = widget.getEditingState()
```

### PDFWidget.prototype.setEditingState(state)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Set whether the widget is in editing state.

When in editing state any changes to the widget value will not
cause any side-effects such as changing other widgets or
running Javascript event handlers. This is intended for, e.g.
when a text widget is interactively having characters typed
into it. Once editing is finished the state should reverted
back, before updating the widget value again.

* **Arguments:**
  * **state** (`boolean`)

```javascript
widget.getEditingState(false)
```

<!-- TODO The text layout object needs to be described. -->

### PDFWidget.prototype.layoutTextWidget()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Layout the value of a text widget. Returns a text layout object.

* **Returns:**
  Object

```javascript
var layout = widget.layoutTextWidget()
```

## Flags

### PDFWidget.prototype.isReadOnly()

If the value is read only and the widget cannot be interacted with.

* **Returns:**
  boolean

```javascript
var isReadOnly = widget.isReadOnly()
```

### PDFWidget.prototype.isMultiline()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is multi-line.

* **Returns:**
  boolean

### PDFWidget.prototype.isPassword()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is a password input.

* **Returns:**
  boolean

### PDFWidget.prototype.isComb()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is a text field laid out in “comb” style (forms where you write one character per square).

* **Returns:**
  boolean

### PDFWidget.prototype.isButton()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “button”, “checkbox” or “radiobutton” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isPushButton()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “button” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isCheckbox()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “checkbox” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isRadioButton()

Return whether the widget is of “radiobutton” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isText()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “text” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isChoice()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “combobox” or “listbox” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isListBox()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “listbox” type.

* **Returns:**
  boolean

### PDFWidget.prototype.isComboBox()

<span class="only_mupdfjs">only&nbsp;mupdf.js</span>

Return whether the widget is of “combobox” type.

* **Returns:**
  boolean

## Signatures

### PDFWidget.prototype.isSigned()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns true if the signature is signed.

* **Returns:**
  boolean

```javascript
var isSigned = widget.isSigned()
```

### PDFWidget.prototype.validateSignature()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns number of updates ago when signature became invalid.
Returns 0 is signature is still valid, 1 if it became
invalid during the last save, etc.

* **Returns:**
  number

```javascript
var validNum = widget.validateSignature()
```

### PDFWidget.prototype.checkCertificate()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns “OK” if signature checked out OK, otherwise a text
string containing an error message, e.g. “Self-signed
certificate.” or “Signature invalidated by change to
document.”, etc.

* **Returns:**
  string

```javascript
var result = widget.checkCertificate()
```

### PDFWidget.prototype.checkDigest()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns “OK” if digest checked out OK, otherwise a text string
containing an error message.

* **Returns:**
  string

```javascript
var result = widget.checkDigest()
```

### PDFWidget.prototype.getSignatory()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns a text string with the distinguished name from a signed
signature, or a text string with an error message.

The returned string follows the format:

`"cn=Name, o=Organization, ou=Organizational Unit,
email=jane.doe@example.com, c=US"`

* **Returns:**
  string

```javascript
var signatory = widget.getSignatory()
```

<!-- TODO document what properties exist in the signatureConfig -->
<!-- TODO PDFPKCS7Signer... what do we do with this? mutool run has it, so better document it. maybe mupdf.js will gain PDF PKCS 7 digests in the future? -->

### PDFWidget.prototype.previewSignature(signer, signatureConfig, image, reason, location)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Return a [Pixmap](Pixmap.md) preview of what the signature would look like
if signed with the given configuration. Reason and location may
be `undefined`, in which case they are not shown.

* **Arguments:**
  * **signer** (`PDFPKCS7Signer`)
  * **signatureConfig** (`Object`)
  * **image** ([`Image`](Image.md#Image))
  * **reason** (`string`)
  * **location** (`string`)
* **Returns:**
  Pixmap

```javascript
var pixmap = widget.previewSignature(
        signer,
        {
                showLabels: true,
                showDate: true
        },
        image,
        "",
        ""
)
```

### PDFWidget.prototype.sign(signer, signatureConfig, image, reason, location)

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Sign the signature with the given configuration. Reason and
location may be `undefined`, in which case they are not shown.

* **Arguments:**
  * **signer** (`PDFPKCS7Signer`)
  * **signatureConfig** (`Object`)
  * **image** ([`Image`](Image.md#Image))
  * **reason** (`string`)
  * **location** (`string`)

```javascript
widget.sign(
        signer,
        {
                showLabels: true,
                showDate: true
        },
        image,
        "",
        ""
)
```

### PDFWidget.prototype.clearSignature()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Clear a signed signature, making it unsigned again.

```javascript
widget.clearSignature()
```

### PDFWidget.prototype.incrementalChangesSinceSigning()

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

Returns true if there have been incremental changes since the
signature widget was signed.

* **Returns:**
  boolean

```javascript
var changed = widget.incrementalChangesSinceSigning()
```
