# Link

Link objects contain information about page links, such as their bounding
box and their destination URI.

To determine whether a link is document-internal or an external link to a web page, see [`Link.prototype.isExternal()`](#Link.prototype.isExternal).
Finally, to resolve a document-internal link see [`Document.prototype.resolveLinkDestination()`](Document.md#Document.prototype.resolveLinkDestination).

## Constructors

### *class* Link()

*You cannot create instances of this class with the new operator!*

To obtain the links on a page see [`Page.prototype.getLinks()`](Page.md#Page.prototype.getLinks).

To create a new link on a page see [`Page.prototype.createLink()`](Page.md#Page.prototype.createLink).

## Instance methods

### Link.prototype.getBounds()

Returns a rectangle describing the link’s location on the page.

* **Returns:**
  [Rect](Rect.md)

```javascript
var rect = link.getBounds()
```

### Link.prototype.setBounds(rect)

Sets the bounds for the link’s location on the page.

* **Arguments:**
  * **rect** ([`Rect`](Rect.md#Rect)) – Desired bounds for link.

```javascript
link.setBounds([0, 0, 100, 100])
```

### Link.prototype.getURI()

Returns a string URI describing the link’s destination. If
[`isExternal()`](#Link.prototype.isExternal) returns `true`, this is a URI suitable for a
browser, if it returns `false` pass it to [`Document.prototype.resolveLink`](Document.md#Document.prototype.resolveLink) to access
to the destination page in the document.

* **Returns:**
  string

```javascript
var uri = link.getURI()
```

### Link.prototype.setURI(uri)

Sets this link’s destination to the given URI. To create links to other
pages within the document, see [`Document.prototype.formatLinkURI()`](Document.md#Document.prototype.formatLinkURI).

* **Arguments:**
  * **uri** (`string`) – An URI describing the desired link destination.

### Link.prototype.isExternal()

Returns a boolean indicating if the link is external or not. URIs whose
link URI has a valid scheme followed by colon, e.g.
`https://example.com` and `mailto:test@example.com`, are defined to
be external.

* **Returns:**
  boolean

```javascript
var isExternal = link.isExternal()
```
