# ZoomButton

`ZoomButton` is Android’s small zoom-in / zoom-out button.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; zoom speed and pressed-state zoom behavior are not wrapped yet.

## Use it when…

Use `ZoomButton` when the screen needs the classic Android zoom button and those APIs can land later.

## How it behaves

`ZoomButton` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `setZoomSpeed(_:)` or inherited `ImageButton` image APIs at this pin.

## Examples

### Place a zoom button

```swift
ZoomButton(id: 3601)
    .padding(h: 8, v: 8)
```

## Initializers

```swift
ZoomButton()
ZoomButton(id: 3601)
```

## Notes

Android’s `ZoomButton` repeatedly zooms while pressed. That behavior is not part of the current Droid surface.
