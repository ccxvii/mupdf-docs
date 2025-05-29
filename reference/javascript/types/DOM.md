# DOM

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

This represents an HTML or an DOM node. It is a helper class intended to
access the DOM (Document Object Model) content of a [Story](Story.md)
object.

## Constructors

### *class* DOM()

*You cannot create instances of this class with the new operator!*

Instances of this class are returned by [`Story.prototype.document()`](Story.md#Story.prototype.document).

## Instance methods

### DOM.prototype.body()

Return a DOM for the body element.

* **Returns:**
  [DOM]()

```javascript
var result = xml.body()
```

### DOM.prototype.documentElement()

Return a DOM for the top level element.

* **Returns:**
  [DOM]()

```javascript
var result = xml.documentElement()
```

### DOM.prototype.createElement(tag)

Create an element with the given tag type, but do not link it into
the [DOM]() yet.

* **Arguments:**
  * **tag** (`string`) – Tag name to use for element.
* **Returns:**
  [DOM]()

```javascript
var result = xml.createElement("div")
```

### DOM.prototype.createTextNode(text)

Create a text node with the given text contents, but do not link
it into the [DOM]() yet.

* **Arguments:**
  * **text** (`string`) – Text contents to put into element.
* **Returns:**
  [DOM]()

```javascript
var result = xml.createElement("Hello world!")
```

### DOM.prototype.find(tag, attribute, value)

Find the first element matching the tag, attribute and value. Set
either of those to `null` to match anything.

* **Arguments:**
  * **tag** (`string`) – The tag of the element to look for.
  * **attribute** (`string`) – The attribute of the element to look for.
  * **value** (`string`) – The value of the attribute of the element to look for.
* **Returns:**
  [DOM]() | null

```javascript
var result = xml.find("tag", "attribute", "value")
```

### DOM.prototype.findNext(tag, attribute, value)

Find the next element matching the tag, attribute and value. Set either
of those to `null` to match anything.

* **Arguments:**
  * **tag** (`string`) – The tag of the element to look for.
  * **attribute** (`string`) – The attribute of the element to look for.
  * **value** (`string`) – The value of the attribute of the element to look for.
* **Returns:**
  [DOM]() | null

```javascript
var result = xml.findNext("tag", "attribute", "value")
```

### DOM.prototype.appendChild(dom, childDom)

Insert an element as the last child of a parent, unlinking the child
from its current position if required.

* **Arguments:**
  * **dom** ([`DOM`](#DOM)) – The DOM that will be parent.
  * **childDom** ([`DOM`](#DOM)) – The DOM that will be a child.

```javascript
xml.appendChild(dom, childDom)
```

### DOM.prototype.insertBefore(dom, elementDom)

Insert an element before this element, unlinking the new element
from its current position if required.

* **Arguments:**
  * **dom** ([`DOM`](#DOM)) – The reference DOM.
  * **elementDom** ([`DOM`](#DOM)) – The DOM that will be inserted before.

```javascript
xml.insertBefore(dom, elementDom)
```

### DOM.prototype.insertAfter(dom, elementDom)

Insert an element after this element, unlinking the new element
from its current position if required.

* **Arguments:**
  * **dom** ([`DOM`](#DOM)) – The reference DOM.
  * **elementDom** ([`DOM`](#DOM)) – The DOM that will be inserted after.

```javascript
xml.insertAfter(dom, elementDom)
```

### DOM.prototype.remove()

Remove this element from the [DOM](). The element can be
added back elsewhere if required.

```javascript
var result = xml.remove()
```

### DOM.prototype.clone()

Clone this element (and its children). The clone is not yet linked
into the [DOM]().

* **Returns:**
  [DOM]()

```javascript
var result = xml.clone()
```

### DOM.prototype.firstChild()

Return the first child of the element as a [DOM](), or
`null` if no child exist.

* **Returns:**
  [DOM]() | null

```javascript
var result = xml.firstChild()
```

### DOM.prototype.parent()

Return the parent of the element as a [DOM](), or `null`
if no parent exists.

* **Returns:**
  [DOM]() | null

```javascript
var result = xml.parent()
```

### DOM.prototype.next()

Return the next element as a [DOM](), or null if no such
element exists.

* **Returns:**
  [DOM]() | null

```javascript
var result = xml.next()
```

### DOM.prototype.previous()

Return the previous element as a [DOM](), or null if no such
element exists.

* **Returns:**
  [DOM]() | null

```javascript
var result = xml.previous()
```

### DOM.prototype.addAttribute(attribute, value)

Add attribute with the given value, returns the updated element as
a DOM.

* **Arguments:**
  * **attribute** (`string`) – Desired attribute name.
  * **value** (`string`) – Desired attribute value.
* **Returns:**
  [DOM]()

```javascript
var result = xml.addAttribute("attribute", "value")
```

### DOM.prototype.removeAttribute(attribute)

Remove the specified attribute from the element, returns the
updated element as a DOM.

* **Arguments:**
  * **attribute** (`string`) – The name of the attribute to remove.
* **Returns:**
  [DOM]()

```javascript
xml.removeAttribute("attribute")
```

### DOM.prototype.getAttribute(attribute)

Return the element’s attribute value as a string, or null if no
such attribute exists.

* **Arguments:**
  * **attribute** (`string`) – The name of the attribute to look up.
* **Returns:**
  string | null

```javascript
var result = xml.attribute("attribute")
```

### DOM.prototype.getAttributes()

Returns a dictionary object with properties and their values
corresponding to the element’s attributes and their values.

* **Returns:**
  Object

```javascript
var dict = xml.getAttributes()
```
