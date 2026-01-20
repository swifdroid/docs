# HSpace

A horizontal spacing helper used to insert empty space with a fixed width or optional weight.

`HSpace` is a convenience wrapper around [`Space`](space.md) that is preconfigured for **horizontal layouts**.  
It is typically used inside `HStack` or other horizontally oriented `LinearLayout` containers.

`HSpace` does not render anything. It only affects layout measurement and positioning.

```swift
body {
    HStack {
        TextView("Left")
        HSpace(16, .dp)
        TextView("Center")
        HSpace(16, .dp)
        Button("Right")
    }
}
```

In this example, `HSpace` creates fixed horizontal gaps between views.