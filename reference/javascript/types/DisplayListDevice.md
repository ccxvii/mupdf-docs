# DisplayListDevice

## Constructors

### *class* DisplayListDevice(displayList)

Create a device for recording onto a display list.

* **Arguments:**
  * **displayList** ([`DisplayList`](DisplayList.md#DisplayList)) – the display list to populate.

```javascript
var myDisplayList = new mupdf.DisplayList([0, 0, 100, 100])
var displayListDevice = new mupdf.DisplayListDevice(myDisplayList)
```
