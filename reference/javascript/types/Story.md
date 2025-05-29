# Story

<span class="only_mutool">only&nbsp;mutool&nbsp;run</span>

## Constructors

### *class* Story(contents, userCSS, em, archive)

Create a new story with the given contents, formatted according to the
provided user-defined CSS and em size, and an archive to lookup images,
etc.

* **Arguments:**
  * **contents** (`string`) – HTML source code. If omitted, a basic minimum is generated.
  * **userCSS** (`string`) – CSS source code. Defaults to the empty string.
  * **em** (`number`) – The default text font size. Default to 12.
  * **archive** ([`Archive`](Archive.md#Archive)) – An archive from which to load resources for rendering. Currently supported resource types are images and text fonts. If omitted, the Story will not try to look up any such data and may thus produce incomplete output. Defaults to null.

```javascript
var story = new mupdf.Story(<contents>, <css>, <em>, <archive>)
```

## Instance methods

### Story.prototype.document()

Return an [DOM](DOM.md) for an unplaced story. This allows adding content before placing the [Story]().

* **Returns:**
  [DOM](DOM.md)

```javascript
var xml = story.document()
```

### Story.prototype.place(rect, flags)

Place (or continue placing) this Story into the supplied rectangle.
Call [`draw()`](#Story.prototype.draw) to draw the placed content before calling [`place()`](#Story.prototype.place) again
to continue placing remaining content.

`more` in the returned object can take either of these values:

* 0 means that all the text was placed successfully.
* 1 means that some text was not placed due not fitting within the height of the rectangle.
* 2 means that some text was not placed due not fitting within the width of the rectangle. For this to be detected `flags` must be set to 1.

* **Arguments:**
  * **rect** ([`Rect`](Rect.md#Rect)) – Rectangle to place the story within.
  * **flags** (`number`) – When set to 1, will detect when a word does not fit in the rectangle, instead `more` set to 2.
* **Returns:**
  `{ filled: Rect, more: number }`

```javascript
do {
        var result = story.place([0, 0, 100, 100])
        // TODO: create device for this bit of story
        story.draw(device, mupdf.Matrix.identity)
        // TODO: close device
} while (result.more)
```

### Story.prototype.draw(device, transform)

Draw the placed Story to the given [Device](Device.md) with the given transform.

* **Arguments:**
  * **device** ([`Device`](Device.md#Device)) – The device
  * **transform** ([`Matrix`](Matrix.md#Matrix)) – The transform matrix.

```javascript
story.draw(device, mupdf.Matrix.identity)
```
