# ImageView

`ImageView` displays image content in the view hierarchy.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work. A `ScaleType` enum is declared, but drawable/resource/URI assignment, tinting, and bounds helpers are not wrapped yet.

## Use it when…

Use `ImageView` for avatars, icons, thumbnails, and illustrations that belong inside a layout.

## How it behaves

`ImageView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `imageDrawable(_:)`, `imageResource(_:)`, `imageURI(_:)`, `imageIcon(_:)`, `imageTintList(_:)`, `adjustViewBounds(_:)`, `scaleType(_:)`, `maxWidth(_:)`, or `maxHeight(_:)` at this pin.

## Examples

### Place an image slot

```swift
ImageView(id: 1801)
    .padding(h: 8, v: 8)
```

### Hide a missing avatar slot

```swift
let avatar = ImageView(id: 1802)
    .visibility(.gone)
```

## Initializers

```swift
ImageView()
ImageView(id: 1801)
```

## Notes

The nested `ImageView.ScaleType` cases exist in the Swift surface at this pin, but no Droid method currently applies a scale type to a live view. Image-source APIs are not available yet.
