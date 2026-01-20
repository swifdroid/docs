# VSpace

A vertical spacing helper used to insert empty space with a fixed height or optional weight.

`VSpace` is a convenience wrapper around [`Space`](space.md) that is preconfigured for **vertical layouts**.  
It is typically used inside `VStack` or other vertically oriented `LinearLayout` containers.

`VSpace` does not render anything. It only affects layout measurement and positioning.

```swift
body {
    VStack {
        TextView("Top")
        VSpace(16, .dp)
        TextView("Center")
        VSpace(16, .dp)
        Button("Bottom")
    }
}
```

In this example, `VSpace` creates fixed vertical gaps between views.