# Recycler View

A powerful and flexible component for displaying large data sets as a scrollable list or grid.

It is optimized for performance by reusing item views that scroll off-screen, reducing memory usage and avoiding unnecessary view creation.

A basic example:
```swift
RecyclerView()
    .layoutManager(.linear(orientation: .vertical))
    .adapter(
        count: 30,
        createView:
            TextView()
                .width(.matchParent)
                .height(.wrapContent)
                .textColor(.black),
        bindView: { view, index in
            view
                .text("Item at position: \(index)")
                .backgroundColor(index % 2 == 0 ? .lightGray : .white)
                .padding()
        }
    )
    .hasFixedSize(true)
```

## Adapter

The adapter is responsible for:

- reporting how many items exist,
- creating item views,
- binding data to reusable views.

You attach an adapter like this:
```swift
RecyclerView().adapter(...)
```

### Count or Items

You can define the data source in two ways.

**Using a fixed count**  
Provide the total number of items:
```swift
RecyclerView().adapter(
    count: 100,
    ...
)
```

**Using items**  
Provide an array (or a reactive state):
```swift
@State var items: [String] = ["Item 1", "Item 2", "Item 3"]
RecyclerView().adapter(
    items: $items,
    ...
)
```
Using `@State` enables automatic updates when the data changes.

### Create View

`createView` defines how item views are created.  
It is called only when a new view instance is needed.

#### Single view type

If all items use the same view, simply return a view instance:
```swift
createView: TextView()
    .width(.matchParent)
    .height(.wrapContent)
```
This view will later be reused and configured in `bindView`.

#### Multiple view types

If your list contains different item layouts, use `viewType`.

You may still return the same view class for all types:
```swift
createView: { viewType in
    TextView()
        .width(.matchParent)
        .height(.wrapContent)
}
```
Or define a type-safe enum that conforms to `Viewable`:
```swift
enum Row: Viewable {
    case title(TextView)
    case rectangle(LinearLayout)

    var view: some View {
        switch self {
        case .title(let v): v
        case .rectangle(let v): v
        }
    }
}
```
Then create views based on the type:
```swift
RecyclerView()
    .adapter(
        items: ...,
        createView: { viewType in
            switch viewType {
            case .title:
                return Row.title(
                    TextView()
                        .width(.matchParent)
                        .height(.wrapContent)
                )
            case .rectangle:
                return Row.rectangle(
                    LinearLayout()
                        .width(.matchParent)
                        .height(100, .dp)
                )
            }
        },
        bindView: ...,
        viewType: { item, position in
            item.isTitle ? .title : .rectangle
        }
    )
```
This approach keeps complex lists clean, readable, and type-safe.

### Bind View

`bindView` connects your data to a reusable view.  
It is called every time a view is reused for a new item.

#### Using count

When using count, you receive the item position:
```swift
bindView: { view, index in
    view.text("Item at position: \(index)")
}
```

#### Using items

When using `items`, you receive both the item and its position:
```swift
bindView: { view, item, position in
    view.text(item)
}
```
If your view is an enum conforming to `Viewable`, you can safely pattern-match:
```swift
bindView: { view, item, position in
    switch view {
    case .title(let textView):
        textView.text(item.title)
    case .rectangle(let linearLayout):
        linearLayout.backgroundColor(item.color)
    }
}
```

### View Type

If your RecyclerView displays multiple layouts, you must define how the view type is determined.

#### Using integers
```swift
viewType: { item, position -> Int in
    position % 2 // Example: two view types based on position
}
```

#### Using a typed enum

For better readability and safety, use a custom type:
```swift
enum RowType: Int, RecyclerView.ViewType {
    case rectangle = 0
    case title = 1
    static func from(rawValue: Int) -> RowType { .init(rawValue: rawValue)! }
}
```
```swift
RecyclerView()
    .adapter(
        items: ...,
        createView: { type in
            switch type {
            case .rectangle:
                return LinearLayout()
            case .title:
                return TextView()
            }
        },
        viewType: { item, position -> RowType in
            item.id % 2 == 0 ? .rectangle : .title
        }
    )
```

## Layout Managers

Layout managers control **how items are positioned and measured**, and when views are recycled.

You can choose from several built-in layout managers or implement your own.

### Default

These options apply to the base RecyclerView behavior, regardless of the specific layout manager used.

#### Item Prefetch Enabled

Controls whether the LayoutManager should be asked to create and bind views outside the visible viewport while the UI thread is idle between frames.

Enabling this can improve scroll performance by preparing views in advance.
```swift
.itemPrefetchEnabled(true)
```

#### Measurement Cache Enabled

Controls whether RecyclerView should use its own measurement cache for child views.

This cache is more aggressive than the default framework cache and can improve performance when item sizes are stable.

```swift
.measurementCacheEnabled(true)
```

### LinearLayoutManager

Displays items in a vertical or horizontal list.

```swift
.layoutManager(
    .linear(orientation: .vertical)
    .reverseLayout()
    .recycleChildrenOnDetach()
    .initialPrefetchItemCount(10)
    .smoothScrollbarEnabled()
    .stackFromEnd()
)
```

Here `.linear(orientation: .vertical)` is a shorthand for:
```swift
LinearLayoutManager(orientation: .vertical)
```

#### Orientation

Defines the direction in which items are laid out.

Possible values:
- `.vertical`
- `.horizontal`

The default orientation is `.vertical`.

```swift
LinearLayoutManager(orientation: .horizontal)
```
or
```swift
LinearLayoutManager().orientation(.horizontal)
```

#### Reverse Layout

Reverses the item traversal and layout order.

Default value is `false`.

```swift
LinearLayoutManager().reverseLayout(true)
```

#### Recycle Children On Detach

Controls whether child views should be recycled when the LayoutManager is detached from the RecyclerView.

Default value is `false`.

```swift
LinearLayoutManager().recycleChildrenOnDetach(true)
```

#### Initial Prefetch Item Count

Sets how many items should be prefetched when this RecyclerView is nested inside another RecyclerView.

This helps improve scroll performance for nested lists.

```swift
LinearLayoutManager().initialPrefetchItemCount(10)
```

#### Smooth Scrollbar Enabled

When enabled, the scrollbar thumb size and position are calculated based on the number of visible pixels in the visible items.

This assumes that all items have similar dimensions.  
If item sizes vary significantly, the scrollbar may appear to change size while scrolling. In that case, disable this option.

```swift
LinearLayoutManager().smoothScrollbarEnabled(false)
```

#### Stack From End

When enabled, items are laid out starting from the end of the container (for example, from the bottom in a vertical list).

This is commonly used for chat or log-style layouts.

```swift
LinearLayoutManager().stackFromEnd(true)
```

### GridLayoutManager

Displays items in a grid with multiple columns or rows.

It inherits from `LinearLayoutManager`, so all of its configuration options are also available.

```swift
.layoutManager(
    .grid(spanCount: 3, orientation: .vertical)
    .reverseLayout(true)
    .usingSpansToEstimateScrollbarDimensions(true)
)
```

#### Span Count

Defines the number of spans in the grid.
- For `.vertical` orientation: number of columns
- For `.horizontal` orientation: number of rows

```swift
GridLayoutManager(spanCount: 3)
```
or
```swift
GridLayoutManager().spanCount(3)
```

#### Using Spans To Estimate Scrollbar Dimensions

When enabled, scrollbar range and offset calculations take span information into account.

This improves scrollbar accuracy but requires additional calls to
SpanSizeLookup.getSpanGroupIndex.

Whether this is necessary depends on the structure and variability of your grid.

```swift
GridLayoutManager().usingSpansToEstimateScrollbarDimensions(true)
```

### StaggeredGridLayoutManager

Displays items in a staggered grid, where items may have different sizes along the scrolling axis.

```swift
.layoutManager(
    .staggeredGrid(spanCount: 3, orientation: .vertical)
    .gapStrategy(.none)
    .reverseLayout(true)
)
```

#### Gap Strategy

Defines how the layout manager handles gaps between items.

Changing this value triggers a layout pass.

Available strategies:
- `.none` – leaves gaps as-is
- `.lazy` – deprecated, no longer supported
- `.moveItemsBetweenSpans` – minimizes gaps by moving items between spans with animations

```swift
StaggeredGridLayoutManager().gapStrategy(.none)
```

#### Reverse Layout

Reverses item traversal and layout order.

Default value is `false`.

```swift
StaggeredGridLayoutManager().reverseLayout(true)
```

### FlexboxLayoutManager

Not implemented yet.  
You can still use Android’s standard `FlexboxLayoutManager` with additional setup. Native support is planned for future releases.

## Has Fixed Size

By default, RecyclerView assumes item sizes may change.  
If the overall size of the RecyclerView **does not depend on its content**, enable fixed size for better performance:

```swift
RecyclerView().hasFixedSize(true)
```

## Data Set and Item Changes

To notify a RecyclerView about data set changes manually, you need a reference to its instance.

!!! important ""
    If you are using items backed by `@State`, you do not need to call any notify methods – updates are handled automatically.

There are two recommended ways to keep a reference to a RecyclerView.

**Using a lazy property**
```swift
lazy var recyclerView = RecyclerView()
```
Use it directly in the body:
```swift
body {
    recyclerView
        .layoutManager(...)
        .adapter(...)
}
```

**Using a bound instance**  
Declare a force-unwrapped variable:
```swift
var recyclerView: RecyclerView!
```
Bind it inside the body using `itself`:
```swift
body {
    RecyclerView()
        .itself(&self.recyclerView)
        .layoutManager(...)
        .adapter(...)
}
```
Once you have a reference, you can explicitly notify the RecyclerView about data changes.

### Data Set Changed

Notifies the adapter that **the entire data set has changed**.

This forces all visible views to be rebound and is similar to `UITableView.reloadData()` in iOS.

```swift
items = ["New", "Data", "Set"]
recyclerView.notifyDataSetChanged()
```
Better UX can be achieved by using more specific notify methods below.

### Item Changed

Notifies that a single item has changed.

```swift
items[0] = "New Value"
recyclerView.notifyItemChanged(at: 0)
```

### Item Removed

Notifies that a single item has been removed.

```swift
items.remove(at: 0)
recyclerView.notifyItemRemoved(at: 0)
```

### Item Inserted

Notifies that a single item has been inserted.

```swift
items.insert("Inserted Item", at: 0)
recyclerView.notifyItemInserted(at: 0)
```

### Item Range Changes

Notifies that a range of items has changed.

```swift
items[0] = "New Value 1"
items[1] = "New Value 2"
recyclerView.notifyItemRangeChanged(startAt: 0, count: 2)
```

### Item Range Removed

Notifies that a range of items has been removed.

```swift
items.removeSubrange(0..<2)
recyclerView.notifyItemRangeRemoved(startAt: 0, count: 2)
```

### Item Range Inserted

Notifies that a range of items has been inserted.

```swift
items.insert(contentsOf: ["Inserted Item 1", "Inserted Item 2"], at: 0)
recyclerView.notifyItemRangeInserted(startAt: 0, count: 2)
```

## Item Size and Performance

RecyclerView is highly optimized, but changing item sizes incorrectly can significantly affect scroll performance.

In general, avoid resizing item views inside `bindView` unless absolutely necessary.

### Prefer Content-Based Sizing (`wrapContent`)

If an item’s height depends on its content (for example, text length or image size), use `.wrapContent` for the root view.

```swift
TextView()
    .width(.matchParent)
    .height(.wrapContent)
```

This allows RecyclerView to measure items efficiently during the layout phase, without triggering extra layout passes while scrolling.

### Use View Types for Categorical Sizes

If your list contains a small set of predefined item sizes (for example: compact, regular, expanded), use viewType and provide different views per type.

```swift
viewType: { item, _ in
    item.isLarge ? .large : .compact
}
```
This lets RecyclerView recycle views of the same size, instead of continuously resizing a single view instance.

### Programmatic Sizing in bindView (Use Sparingly)

If item size must be calculated dynamically (for example, based on screen size), you may set the height in `bindView`:

```swift
bindView: { view, item, _ in
    view.height(calculatedHeight, .dp)
}
```

!!! warning ""
    Only update the size when it actually changes. Repeated layout updates trigger expensive layout passes and can cause scroll stutter.

### `hasFixedSize` and Variable Item Heights

If your items do not all have the same size, ensure fixed size mode is disabled:
```swift
RecyclerView().hasFixedSize(false)
```
When `hasFixedSize(true)` is enabled, RecyclerView assumes all items have identical dimensions, which can cause visual issues with variable-sized items.

## Common Patterns

### Empty State

RecyclerView does not provide built-in empty state handling.

Unlike SwiftUI, SwifDroid does **not** recreate the view hierarchy when state changes.  
Instead, view instances observe `@State` and update their properties reactively.

The recommended approach is to keep both views in the hierarchy and control their visibility based on state.

```swift
@State var items: [String] = []

body {
    TextView("No items available")
        .gravity(.center)
        .width(.matchParent)
        .height(.matchParent)
        .visibility($items.map { $0.isEmpty ? .visible : .gone })
    RecyclerView()
        .adapter(
            items: $items,
            createView: TextView(),
            bindView: { view, item, _ in
                view.text(item)
            }
        )
        .visibility($items.map { $0.isEmpty ? .gone : .visible })
}
```