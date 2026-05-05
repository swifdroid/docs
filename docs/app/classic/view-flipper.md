# ViewFlipper

`ViewFlipper` is Android’s auto-flipping child container.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; flip intervals, auto-start, and child switching are not wrapped yet.

## Use it when…

Use `ViewFlipper` when the screen needs the classic Android flipper control and those APIs can land later.

## How it behaves

`ViewFlipper` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `ViewAnimator` APIs and does not expose `setFlipInterval(_:)`, `startFlipping()`, `stopFlipping()`, or child-switch helpers at this pin.

## Examples

### Place a flipper

```swift
ViewFlipper(id: 3101)
    .padding(h: 0, v: 0)
```

## Initializers

```swift
ViewFlipper()
ViewFlipper(id: 3101)
```

## Notes

Android’s `ViewFlipper` rotates children on a timer. That surface is not part of the current Droid API.
