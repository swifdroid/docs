# HorizontalScrollView

`HorizontalScrollView` provides a horizontally scrolling viewport for content wider than the screen. It is useful for compact rows of controls, images, filters, or other content that should stay on one horizontal axis.

Keep one direct child inside the scroll view, usually a horizontal `LinearLayout` that owns the actual row content.

## Use it when…

Use `HorizontalScrollView` when the content is modest in size but may not fit within the available width.

For large or recycled collections, prefer [`RecyclerView`](../x/recycler-view.md) configured for horizontal layout instead of building an oversized static row.

## How it behaves

The scroll view clips its child to a horizontal viewport and lets the user pan left and right. Programmatic scrolling, flinging, and scroll-state reads operate on the live Android instance.

`fillViewport(_:)`, `smoothScrollingEnabled(_:)`, and the edge-effect color setters can be configured before realization. The viewport and smooth-scrolling zero-argument methods read live state, while `leftEdgeEffectColor()` and `rightEdgeEffectColor()` read the live edge colors.

Horizontal gestures can compete with vertical scrolling in a parent screen, so keep interactive items comfortably sized and avoid unnecessary nested scroll layers.

## Examples

### Build a horizontally scrollable filter row

```swift
HorizontalScrollView {
    LinearLayout {
        Button("All")
        Button("Unread")
        Button("Mentions")
        Button("Archived")
    }
    .orientation(.horizontal)
    .padding(16)
}
```

The buttons stay in one row and the viewport scrolls horizontally when the row is wider than the screen.

### Fill the viewport when content is short

```swift
HorizontalScrollView {
    LinearLayout {
        TextView("Overview")
        TextView("Details")
    }
    .orientation(.horizontal)
}
.fillViewport(true)
```

`fillViewport(true)` stretches short content to at least the viewport width while still allowing horizontal scrolling when the child becomes wider.

### Scroll a realized view to a saved position

```swift
func restorePosition(_ scrollView: HorizontalScrollView, x: Int) {
    scrollView.scrollTo(x: x, y: 0)
}
```

`scrollTo(x:y:_:)` is a live operation, so call it after the Android view exists.

## Initializers

Create an empty scroll view, provide its child directly, or assign an id when you need a stable reference:

```swift
HorizontalScrollView()

HorizontalScrollView {
    LinearLayout {
        TextView("Scrollable content")
    }
    .orientation(.horizontal)
}

HorizontalScrollView(id: 1001)
```

## Methods

### Jump to a position

`scrollTo(x:y:_:)` moves the live viewport immediately. Coordinates use `.dp` by default.

```swift
scrollView.scrollTo(x: 240, y: 0)
```

Pass another `DimensionUnit` when the input is not in dp.

### Smoothly move the viewport

`smoothScrollBy(dx:dy:_:)` moves relative to the current position, while `smoothScrollTo(x:y:_:)` moves toward an absolute position.

```swift
scrollView.smoothScrollBy(dx: 120, dy: 0)

scrollView.smoothScrollTo(x: 360, y: 0)
```

Both are live operations.

### Start a fling

`fling(_:)` starts horizontal momentum using the supplied X velocity.

```swift
scrollView.fling(1800)
```

Use this when your own interaction needs to hand velocity to the Android scroll view.

### Fill the viewport

`fillViewport(_:)` configures whether short content should stretch to the viewport width. `fillViewport()` reads the live value.

```swift
scrollView.fillViewport(true)

let fillsWidth = scrollView.fillViewport()
```

The fluent setter can be queued before realization; the getter needs the live instance.

### Control smooth arrow scrolling

`smoothScrollingEnabled(_:)` configures whether arrow-style scrolling animates its transition. The zero-argument method reads the live state.

```swift
scrollView.smoothScrollingEnabled(false)

let smooth = scrollView.smoothScrollingEnabled()
```

### Read the maximum scroll amount

`maxScrollAmount(_:)` reads Android's current maximum arrow-scroll amount and converts it to the requested unit.

```swift
let amount = scrollView.maxScrollAmount()
let pixels = scrollView.maxScrollAmount(.px)
```

### Configure edge effects

`edgeEffectColor(_:)` sets both horizontal edge effects. `leftEdgeEffectColor(_:)` and `rightEdgeEffectColor(_:)` configure them separately.

```swift
let edgeColor = 0x7f3f51b5

scrollView
    .leftEdgeEffectColor(edgeColor)
    .rightEdgeEffectColor(edgeColor)
```

Read the live colors with `leftEdgeEffectColor()` and `rightEdgeEffectColor()`.

### Direct directional scrolling

`arrowScroll(_:)`, `pageScroll(_:)`, and `fullScroll(_:)` expose Android's live directional scroll actions.

```swift
let moved = scrollView.pageScroll(direction)
```

These lower-level methods are most useful when application code already has the Android direction value it wants to forward.
