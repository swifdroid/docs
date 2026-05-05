# TextSwitcher

`TextSwitcher` is Android’s two-view control for switching between text values.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; factory bridges, text setters, and switcher animations are not wrapped yet.

## Use it when…

Use `TextSwitcher` when the screen needs the classic Android text-switching control and those APIs can land later.

For a single text slot, use [`TextView`](text-view.md).

## How it behaves

`TextSwitcher` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `ViewSwitcher` / `ViewAnimator` APIs and does not expose `setText(_:)`, `factory(_:)`, or animation helpers at this pin.

## Examples

### Place a text switcher

```swift
TextSwitcher(id: 2901)
    .padding(h: 16, v: 8)
```

## Initializers

```swift
TextSwitcher()
TextSwitcher(id: 2901)
```

## Notes

Android’s `TextSwitcher` alternates two text views with transitions. That surface is not part of the current Droid API.
