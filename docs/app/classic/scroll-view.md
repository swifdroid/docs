# ScrollView

`ScrollView` provides a vertically scrolling viewport for content taller than the screen. It is a good fit for forms, settings, articles, and other screens whose content is finite but may extend beyond the available height.

Keep one direct child inside the scroll view, usually a vertical `LinearLayout` that owns the screen content.

## Use it when…

Use `ScrollView` when the screen has a moderate amount of non-recycled content that should move together vertically.

For large repeating collections, use [`RecyclerView`](../x/recycler-view.md) instead of placing many rows in one long static hierarchy.

## How it behaves

The scroll view clips its child to a vertical viewport and lets the user pan up and down. Programmatic scrolling, flinging, and scroll-state reads operate on the live Android instance.

`fillViewport(_:)`, `smoothScrollingEnabled(_:)`, and the edge-effect color setters can be configured before realization. The viewport and smooth-scrolling zero-argument methods read live state, while `topEdgeEffectColor()` and `bottomEdgeEffectColor()` read the live edge colors.

Nested scrolling hooks and touch/layout methods are also exposed for lower-level integration, but ordinary screens usually only need the scrolling and configuration methods documented below.

## Examples

### Build a scrollable settings screen

```swift
ScrollView {
    LinearLayout {
        TextView("Account")
            .textSize(20)

        EditText()
            .hint("Display name")

        EditText()
            .hint("Email")

        Button("Save")
    }
    .orientation(.vertical)
    .padding(16)
}
```

The whole form scrolls as one vertical surface when it grows beyond the viewport.

### Fill the viewport for short content

```swift
ScrollView {
    LinearLayout {
        TextView("Nothing else to configure")
    }
    .orientation(.vertical)
}
.fillViewport(true)
```

`fillViewport(true)` makes short content fill at least the viewport height while keeping normal vertical scrolling when the content grows.

### Restore a live scroll position

```swift
func restorePosition(_ scrollView: ScrollView, y: Int) {
    scrollView.scrollTo(x: 0, y: y)
}
```

`scrollTo(x:y:_:)` is a live operation, so call it after the Android view exists.

## Initializers

Create an empty scroll view, provide its child directly, or assign an id for later access:

```swift
ScrollView()

ScrollView {
    LinearLayout {
        TextView("Scrollable content")
    }
    .orientation(.vertical)
}

ScrollView(id: 1001)
```

## Methods

### Jump to a position

`scrollTo(x:y:_:)` moves the live viewport immediately. Coordinates use `.dp` by default.

```swift
scrollView.scrollTo(x: 0, y: 320)
```

Pass another `DimensionUnit` when the input is not in dp.

### Smoothly move the viewport

`smoothScrollBy(dx:dy:_:)` moves relative to the current position, while `smoothScrollTo(x:y:_:)` moves toward an absolute position.

```swift
scrollView.smoothScrollBy(dx: 0, dy: 160)

scrollView.smoothScrollTo(x: 0, y: 640)
```

Both are live operations.

### Start a fling

`fling(_:)` starts vertical momentum using the supplied Y velocity.

```swift
scrollView.fling(2200)
```

Use this when custom interaction code needs to pass velocity into the scroll view.

### Fill the viewport

`fillViewport(_:)` configures whether short content should stretch to the viewport height. `fillViewport()` reads the live value.

```swift
scrollView.fillViewport(true)

let fillsHeight = scrollView.fillViewport()
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

`edgeEffectColor(_:)` sets both vertical edge effects. `topEdgeEffectColor(_:)` and `bottomEdgeEffectColor(_:)` configure them separately.

```swift
let edgeColor = 0x7f3f51b5

scrollView
    .topEdgeEffectColor(edgeColor)
    .bottomEdgeEffectColor(edgeColor)
```

Read the live colors with `topEdgeEffectColor()` and `bottomEdgeEffectColor()`.

### Direct directional scrolling

`arrowScroll(_:)`, `pageScroll(_:)`, and `fullScroll(_:)` expose Android's live directional scroll actions.

```swift
let moved = scrollView.fullScroll(direction)
```

These lower-level methods are most useful when application code already has the Android direction value it wants to forward.

### Nested scrolling hooks

The current API also exposes Android's nested-scroll callbacks such as `onStartNestedScroll(_:_:_:)`, `onNestedScrollAccepted(_:_:_:)`, `onNestedScroll(_:_:_:_:_:)`, `onNestedFling(_:_:_:_:)`, and `onStopNestedScroll(_:)`.

These are live integration hooks for custom scrolling behavior. Most application screens can leave them to Android's normal view lifecycle.
