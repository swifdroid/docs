# ZoomControls

`ZoomControls` is Android’s paired zoom-in / zoom-out control.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; zoom listeners and visibility helpers are not wrapped yet.

## Use it when…

Use `ZoomControls` when the screen needs the classic Android zoom pair and those APIs can land later.

## How it behaves

`ZoomControls` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `setOnZoomInClickListener(_:)`, `setOnZoomOutClickListener(_:)`, `setZoomSpeed(_:)`, or `show()` / `hide()` fade helpers at this pin.

## Examples

### Place zoom controls

```swift
ZoomControls(id: 3701)
    .padding(h: 8, v: 8)
```

## Initializers

```swift
ZoomControls()
ZoomControls(id: 3701)
```

## Notes

Android’s `ZoomControls` hosts two zoom buttons and optional auto-hide. That surface is not part of the current Droid API.
