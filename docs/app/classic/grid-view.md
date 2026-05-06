# GridView

`GridView` is Android’s two-dimensional adapter-backed grid.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; column count, spacing, stretch mode, and adapter APIs are not wrapped yet.

## Use it when…

Use `GridView` when the screen needs the classic Android grid widget and will grow into adapter-backed cells later.

## How it behaves

`GridView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `numColumns(_:)`, `horizontalSpacing(_:)`, `verticalSpacing(_:)`, `stretchMode(_:)`, `adapter(_:)`, or selection helpers at this pin.

## Examples

### Place a grid container

```swift
GridView(id: 2201)
    .padding(h: 8, v: 8)
```

## Initializers

```swift
GridView()
GridView(id: 2201)
```

## Notes

Android’s `GridView` is defined by columns, spacing, and its adapter. None of that is part of the current Droid surface.
