# ScrollView

`ScrollView` is the classic Android scrolling container for one scrolling child.

In the current Droid source pin it is a `ViewGroup` with a single-child content initializer. You can build a scrolling stack through the hierarchy APIs. Scroll-state callbacks, smooth scrolling, and fillViewport-style controls are not wrapped yet.

## Use it when…

Use `ScrollView` when a short screenful of content may grow taller than the device and should scroll as one block.

Do not put a vertically scrolling list widget inside it; use one scrolling strategy per axis.

## How it behaves

`ScrollView` accepts declarative single-child content and participates in the view hierarchy. Shared view configuration such as `onClick(_:)`, padding, `enabled(_:)`, and `visibility(_:)` remains available.

The current type does **not** expose `smoothScrollTo(_:_:)`, `fullScroll(_:)`, `arrowScroll(_:)`, `isFillViewport()`, or scroll-change listeners at this pin.

## Examples

### Scroll a settings column

```swift
ScrollView(id: 2301) {
    LinearLayout(id: 2302) {
        TextView("Account")
        TextView("Privacy")
        TextView("About")
    }
}
```

The column becomes scrollable when it is taller than the viewport.

## Initializers

```swift
ScrollView()
ScrollView(id: 2301)

ScrollView(id: 2301) {
    // single child content via BodyBuilder
}
```

## Layout params

Children use the shared view layout-param surface. The scroll view itself uses frame-style layout params at this pin.

## Notes

Smooth-scroll and fill-viewport helpers from Android’s `ScrollView` are not part of the current Droid API. Place one primary scrolling child and size it through normal layout.
