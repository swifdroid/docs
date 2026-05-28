# RecyclerView

`RecyclerView` shows a scrollable collection of item views. A layout manager selects their arrangement; an adapter supplies the item count, creates views, and binds each view to its data.

## Use it when…

Use it for a list or grid whose rows come from a collection, such as a catalogue, message list, or search results. Choose a layout manager for the arrangement and keep the adapter's data consistent with the changes you report.

## How it behaves

`RecyclerView.adapter(...)` builds a `RecyclerViewAdapter` behind the scenes. Its item views conform to `Viewable`; `View` already conforms, so a `TextView` can be used directly. `createView` supplies a view and `bindView` receives that view and an item position. The item-data overload also passes the item itself.

The nested `RecyclerView.Adapter` is the underlying adapter slot/type used when attaching this generic Swift adapter. Use the generic route shown here.

An ordinary `items:` array expression is evaluated through an escaping autoclosure when the adapter reads its count or an item. Mutating that array does not emit notifications. Keep the data in storage that the closure can read, then notify the matching change.

## Examples

This list displays thirty numbered rows:

```swift
let recyclerView = RecyclerView()
    .layoutManager(.linear(orientation: .vertical))
    .adapter(
        count: 30,
        createView: TextView(),
        bindView: { view, index in
            view.textKeepState("Item \(index + 1)")
        }
    )
```

For mutable item data, keep both the array and the view on an owning object. These properties display the current destination names and support the notification examples below:

```swift
@MainActor
final class DestinationList {
    var items = ["Yerevan", "Gyumri", "Dilijan"]

    lazy var recyclerView = RecyclerView()
        .layoutManager(.linear(orientation: .vertical))
        .adapter(
            items: self.items,
            createView: TextView(),
            bindView: { view, item, _ in
                view.textKeepState(item)
            }
        )
}
```

## Methods

### Adapter inputs

The `count:` overload binds by index. The ordinary `items:` array overload also passes each item to `bindView`. Both forms accept a view expression or a zero-argument creation closure:

```swift
RecyclerView()
    .layoutManager(.linear(orientation: .vertical))
    .adapter(
        count: 10,
        createView: { TextView() },
        bindView: { view, index in
            view.textKeepState("Result \(index + 1)")
        }
    )
```

### Layout managers

`layoutManager(_:)` accepts a `LayoutManager`. The base type exposes `itemPrefetchEnabled(_:)`, which controls whether it is queried for views outside its viewport while the UI thread is idle between frames. Configure it on the selected manager:

```swift
RecyclerView().layoutManager(
    LinearLayoutManager(orientation: .vertical)
        .itemPrefetchEnabled(true)
)
```

`LayoutManager.linear`, `.grid`, and `.staggeredGrid` are convenience factories for the corresponding concrete managers. Specify `.vertical` or `.horizontal` explicitly when direction matters.

#### LinearLayoutManager

Use a linear manager for a vertical or horizontal list. Pass direction to its initializer; `reverseLayout(_:)` reverses traversal and layout order, and `stackFromEnd(_:)` fills from the opposite end, such as the bottom of a vertical list.

```swift
RecyclerView().layoutManager(
    .linear(orientation: .vertical)
        .reverseLayout()
        .stackFromEnd(true)
)
```

`recycleChildrenOnDetach(_:)` controls whether children are recycled when the manager detaches. `initialPrefetchItemCount(_:)` sets the number of inner items to prefetch when this list is nested in another RecyclerView. `smoothScrollbarEnabled(false)` avoids pixel-based scrollbar calculations that assume similarly sized items.

```swift
LinearLayoutManager(orientation: .horizontal)
    .recycleChildrenOnDetach(true)
    .initialPrefetchItemCount(4)
    .smoothScrollbarEnabled(false)
```

#### GridLayoutManager

A grid manager takes a span count. For vertical orientation, spans are columns; for horizontal orientation, they are rows. It inherits the linear manager configuration methods.

```swift
let grid = GridLayoutManager(spanCount: 3, orientation: .vertical)
RecyclerView().layoutManager(grid.usingSpansToEstimateScrollbarDimensions(true))
```

`usingSpansToEstimateScrollbarDimensions(_:)` includes span information in scroll offset and range calculations.

#### StaggeredGridLayoutManager

The staggered manager requires both span count and orientation. `reverseLayout(_:)` reverses traversal and layout order. `gapStrategy(_:)` selects how gaps are handled; changing the strategy requests layout.

```swift
RecyclerView().layoutManager(
    .staggeredGrid(spanCount: 2, orientation: .vertical)
        .gapStrategy(.moveItemsBetweenSpans)
        .reverseLayout()
)
```

`.none` leaves gaps alone. `.moveItemsBetweenSpans` checks for gaps when scrolling becomes idle and can move items to correct them. `.lazy` is marked deprecated and unsupported.

#### FlexboxLayoutManager

Configure Flexbox direction and wrapping through the initializer, then set alignment with `alignItems(_:)`:

```swift
RecyclerView().layoutManager(
    FlexboxLayoutManager(flexDirection: .row, flexWrap: .wrap)
        .alignItems(.flexStart)
)
```

`flexDirection(_:)` and `flexWrap(_:)` also expose those settings as methods. Direction cases are `.row`, `.rowReverse`, `.column`, and `.columnReverse`; wrapping cases are `.noWrap`, `.wrap`, and `.wrapReverse`. Alignment cases are `.flexStart`, `.flexEnd`, `.center`, `.baseline`, and `.stretch`.

```swift
RecyclerView().layoutManager(
    FlexboxLayoutManager()
        .flexDirection(.column)
        .flexWrap(.noWrap)
        .alignItems(.center)
)
```

### Fixed item size

Use `hasFixedSize(true)` only when all item views keep an identical fixed size in the scrolling direction: height for a vertical list or width for a horizontal list. For content-dependent sizes, keep it disabled:

```swift
recyclerView.hasFixedSize(false)
```

### Report item changes

For the ordinary array example, the following snippets are separate operations inside `DestinationList` methods. Mutate the data first, then report the same positions and counts. Use precise notifications when you know the change; these calls forward to the attached adapter and do not mutate your data.

Insert or remove one item with `notifyItemInserted(at:)` or `notifyItemRemoved(at:)`:

```swift
items.insert("Vanadzor", at: 1)
recyclerView.notifyItemInserted(at: 1)

items.remove(at: 1)
recyclerView.notifyItemRemoved(at: 1)
```

Report a changed item with `notifyItemChanged(at:)`, or a move with `notifyItemMoved(fromPosition:toPosition:)`:

```swift
items[0] = "Yerevan centre"
recyclerView.notifyItemChanged(at: 0)

let moved = items.remove(at: 0)
items.insert(moved, at: 2)
recyclerView.notifyItemMoved(fromPosition: 0, toPosition: 2)
```

For contiguous changes, use the corresponding range notification:

```swift
items.insert(contentsOf: ["Sevan", "Goris"], at: 0)
recyclerView.notifyItemRangeInserted(startAt: 0, count: 2)

items[0] = "Lake Sevan"
items[1] = "Goris centre"
recyclerView.notifyItemRangeChanged(startAt: 0, count: 2)

items.removeSubrange(0..<2)
recyclerView.notifyItemRangeRemoved(startAt: 0, count: 2)
```

When replacing the entire collection without a precise change description, `notifyDataSetChanged()` reports a full refresh:

```swift
items = ["Kapan", "Meghri"]
recyclerView.notifyDataSetChanged()
```

### Paging helper

`paginable()` constructs a `PagerSnapHelper` and calls its `attachToRecyclerView` method:

```swift
recyclerView.paginable()
```

## Notes

Paging is unverified at this source revision: `PagerSnapHelper.attachToRecyclerView` signs the view argument as `PagerSnapHelper.className`. The exposed call therefore does not establish that page snapping works at runtime.
