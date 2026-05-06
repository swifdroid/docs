# StackView

`StackView` is Android’s stacked-card browser for one item at a time.

In the current Droid source pin it is a placeable view with a single-child content initializer. Shared view APIs work; adapter-backed stacking, advance/defer helpers, and stack animations are not wrapped yet.

## Use it when…

Use `StackView` when the screen should show a deck-like control and the adapter surface can come later.

## How it behaves

`StackView` can take declarative single-child content and participates in the view hierarchy. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `adapter(_:)`, `showNext()`, `showPrevious()`, or stack-mode APIs at this pin.

## Examples

### Place a stack control

```swift
StackView(id: 2501) {
    TextView("Card")
}
```

## Initializers

```swift
StackView()
StackView(id: 2501)

StackView(id: 2501) {
    // single child content via BodyBuilder
}
```

## Notes

Android’s `StackView` is defined by its adapter and stack navigation. None of that is part of the current Droid surface.
