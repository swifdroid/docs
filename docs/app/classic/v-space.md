# VSpace

`VSpace` inserts an empty vertical gap with a chosen height. Calling `VSpace(16)` creates a 16 dp spacer by default, which makes repeated vertical rhythm easy to express directly in a `VStack`.

Unlike [`Space`](space.md), `VSpace` starts with weight `0`. Its normal role is therefore a fixed vertical separation, not a flexible spacer that immediately consumes leftover room.

## Use it when…

Use `VSpace` between stacked sections whose separation should stay deliberate as the screen changes: a heading and its content, two form groups, or a control and the content that follows it.

Use `Space()` when the empty area should stretch to fill what remains, such as keeping a button near the bottom of a taller column.

## How it behaves

The height is required and may be supplied as an integer dimension or a layout-size value. The unit defaults to `.dp`, so the common fixed-gap form stays compact.

The default weight is `0`. Supplying a nonzero `weight:` lets the spacer participate in compatible vertical weight distribution, allowing the parent layout to give it a share of additional available height.

## Examples

### Keep sections evenly separated

The same fixed spacer can be repeated to give a column a consistent vertical rhythm:

```swift
body {
    VStack {
        TextView("Account")
        VSpace(16)
        TextView("Signed in")
        VSpace(16)
        Button("Continue")
    }
}
```

### Allow a vertical gap to grow

Here the spacer has an explicit height and also participates in the column's weight distribution:

```swift
body {
    VStack {
        TextView("Header")
        VSpace(8, weight: 1)
        Button("Continue")
    }
    .height(.matchParent)
}
```

If the intended behavior is simply to consume the remaining height, [`Space`](space.md) is usually the clearer choice because it starts with weight `1`.
