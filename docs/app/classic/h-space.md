# HSpace

`HSpace` creates an empty horizontal gap with an explicit width. `HSpace(16)`, for example, means a 16 dp gap because the unit defaults to `.dp`.

Its default weight is `0`, so it behaves as a fixed-width spacer unless you deliberately give it weight. Use [`Space`](space.md) when the gap should be flexible by default rather than start from a chosen width.

## Use it when…

Use `HSpace` in a row when the horizontal distance between neighboring views is part of the design: between an icon and a label, between buttons, or between compact groups that should keep a predictable separation.

Choose `Space()` instead when the important behavior is "consume the room that is left" rather than "leave this much room here."

## How it behaves

The width is required. You can provide it as an integer dimension or as a layout-size value, and the unit defaults to `.dp`.

Weight defaults to `0`. If you pass a nonzero `weight:`, the spacer also participates in the parent's compatible horizontal weight distribution; the final allocation then depends on that layout rather than on the width alone.

## Examples

### Keep a 16 dp gap

No unit is needed for the common dp case:

```swift
body {
    HStack {
        TextView("Filter")
        HSpace(16)
        Button("Apply")
    }
}
```

The label and button remain separated by the explicit horizontal spacer instead of relying on either view's padding.

### Let the gap participate in weight distribution

A weighted `HSpace` is useful when you still want to declare a horizontal dimension while allowing a compatible parent to distribute extra room by weight.

```swift
body {
    HStack {
        TextView("Details")
        HSpace(8, weight: 1)
        Button("Open")
    }
    .width(.matchParent)
}
```

For a spacer whose normal job is to absorb remaining room, [`Space`](space.md) communicates that intent more directly.
