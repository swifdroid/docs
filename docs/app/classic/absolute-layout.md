# AbsoluteLayout

`AbsoluteLayout` positions children using explicit X and Y coordinates.

Android deprecates this layout because fixed coordinates do not adapt well to different screen sizes, font sizes, or layout directions. Prefer flexible layouts such as `FrameLayout` for overlays, or a constraint-based layout for relationships between views.

## Use it when…

Use `AbsoluteLayout` only for legacy UI or tightly controlled fixed-coordinate layouts where adapting to different device configurations is not a requirement.

## How it behaves

Each child receives its position through the `x(_:_:)` and `y(_:_:)` layout params. Both use `.dp` by default, so the values are converted using the current display density.

The coordinates remain fixed relative to the parent. If content size or available space changes, `AbsoluteLayout` does not automatically reflow the children.

## Examples

### Place a label at a fixed position

```swift
AbsoluteLayout {
    TextView("Tap here")
        .x(16)
        .y(24)
}
.width(.matchParent)
.height(160)
.backgroundColor(.lightGray)
```

The label starts 16 dp from the left and 24 dp from the top of the layout.

### Position two children independently

```swift
AbsoluteLayout {
    TextView("A")
        .x(16)
        .y(16)

    TextView("B")
        .x(120)
        .y(60)
}
.height(120)
```

Each child keeps the coordinates assigned to it rather than participating in a row, column, or relative positioning rule.

## Initializers

Create an empty layout, assign an id when you need one, or provide its children directly:

```swift
AbsoluteLayout()
AbsoluteLayout(id: 1001)

AbsoluteLayout {
    TextView("Child")
}
```

## Layout params

Use `x(_:_:)` and `y(_:_:)` on children. Pass a different `DimensionUnit` only when the coordinate should not use the default `.dp`.

```swift
TextView("Hello")
    .x(24)
    .y(24)
```

## Notes

Avoid `AbsoluteLayout` for responsive screens. Fixed coordinates are difficult to maintain when available space, text size, or layout direction changes.
