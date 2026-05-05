# ViewAnimator

`ViewAnimator` is Android’s base container for switching among child views.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; child factories, displayed-child control, and in/out animations are not wrapped yet.

## Use it when…

Use `ViewAnimator` when you need the classic Android animator container name and the switching APIs can land later.

## How it behaves

`ViewAnimator` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `FrameLayout` and does not expose `setFactory(_:)`, `showNext()`, `showPrevious()`, `setInAnimation(_:)`, or `setOutAnimation(_:)` at this pin.

## Examples

### Place an animator container

```swift
ViewAnimator(id: 3001)
    .padding(h: 0, v: 0)
```

## Initializers

```swift
ViewAnimator()
ViewAnimator(id: 3001)
```

## Notes

Android’s `ViewAnimator` switches children with enter/exit animations. None of that is part of the current Droid surface.
