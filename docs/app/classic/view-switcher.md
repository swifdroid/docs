# ViewSwitcher

`ViewSwitcher` is Android’s two-child switching container.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; factory bridges, next/previous switching, and animations are not wrapped yet.

## Use it when…

Use `ViewSwitcher` when the screen needs the classic Android two-view switcher and those APIs can land later.

## How it behaves

`ViewSwitcher` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `ViewAnimator` APIs and does not expose `setFactory(_:)`, `showNext()`, `showPrevious()`, or animation setters at this pin.

## Examples

### Place a switcher

```swift
ViewSwitcher(id: 3201)
    .padding(h: 0, v: 0)
```

## Initializers

```swift
ViewSwitcher()
ViewSwitcher(id: 3201)
```

## Notes

Android’s `ViewSwitcher` alternates exactly two children. That surface is not part of the current Droid API.
