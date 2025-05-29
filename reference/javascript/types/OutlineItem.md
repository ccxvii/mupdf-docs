# OutlineItem

Outline items are returned from the [`Document.prototype.loadOutline`](Document.md#Document.prototype.loadOutline) method and
represent a table of contents entry.

They are also used with the [OutlineIterator](OutlineIterator.md) interface.

## Constructors

### *class* OutlineItem()

*This is an interface, not a concrete class!*

Outline items are passed around as plain objects.

```javascript
interface OutlineItem {
        title: string | undefined,
        uri: string | undefined,
        open: boolean,
        down?: OutlineItem[],
        page?: number,
}
```
