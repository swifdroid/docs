# Space

`Space()` creates flexible empty room inside a layout. Use it when two pieces of content should move apart and the container, rather than a hard-coded gap, should decide how much room belongs between them.

A new `Space()` starts with weight `1`. In a compatible weight-aware linear layout such as `HStack` or `VStack`, that makes the spacer eligible to take remaining space. When the gap has a deliberate horizontal or vertical size instead, use [`HSpace`](h-space.md) or [`VSpace`](v-space.md).

## Use it when…

Use `Space()` between views that should stay anchored to opposite sides of the available room: a title and an action, a leading value and a trailing control, or two groups that should separate as the screen grows.

If the design calls for a specific gap such as 8 or 16 dp, prefer `HSpace` or `VSpace`. They make the direction and fixed dimension explicit; `Space` is the flexible choice.

## How it behaves

`Space` contributes layout space without visible content. Its public initializer applies weight `1`, so one spacer can absorb the remaining room in a compatible weighted parent.

When a layout contains more than one `Space()`, each starts with the same weight `1`. They therefore participate in the parent's flexible-space distribution with equal weight.

## Examples

### Push an action to the other side

Here the spacer expands between the title and button. As the row gets wider or narrower, the middle gap changes while the content stays at opposite sides.

```swift
body {
    HStack {
        TextView("Inbox")
        Space()
        Button("Compose")
    }
    .width(.matchParent)
    .height(.wrapContent)
}
```

### Split flexible space evenly

Both spacers start with weight `1`, so they participate equally in distribution of the row's remaining flexible space.

```swift
body {
    HStack {
        TextView("Start")
        Space()
        TextView("Middle")
        Space()
        TextView("End")
    }
    .width(.matchParent)
    .height(.wrapContent)
}
```

## Notes

When declaring a new spacer inline, keep `Space()` at its default weight. Chaining `.weight(...)` onto a newly constructed `Space()` does not replace that initializer-applied weight in the current API.

If a directional spacer needs an explicit construction-time weight, `HSpace` and `VSpace` expose `weight:` in their public initializers.
