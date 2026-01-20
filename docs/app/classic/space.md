# Space

An invisible layout helper used to create adjustable gaps between views.

`Space` is a lightweight wrapper around Android’s `android.widget.Space`.  
It does **not** draw anything and exists purely to participate in layout measurement and positioning.

Unlike margins or padding, `Space` behaves like a real view and can expand or shrink based on layout rules such as weight.

```swift
body {
    HStack {
        TextView("Left")
        Space()
        Button("Right")
    }
    .width(.matchParent)
    .height(.wrapContent)
}
```

In this example, Space expands to fill the available horizontal space, pushing the views apart.


## Behavior

- `Space` has **no visual representation**
- It takes up space during layout measurement
- It is ignored by accessibility services
- It is commonly used inside `LinearLayout`-based containers (`HStack`, `VStack`)

By default, `Space` is initialized with:

```swift
weight(1)
```
This means it will expand to consume available space when used inside a layout that supports weights.

## Weight-based spacing

You can control how much space is taken by providing different weights:

```swift
body {
    HStack {
        TextView("A")
        Space(weight: 1)
        TextView("B")
        Space(weight: 2)
        TextView("C")
    }
}
```

In this layout:
- The second space will be twice as wide as the first
- All spacing is resolved by the layout system, not manually calculated

## Fixed-size spacing

If you need a fixed gap instead of flexible spacing, you can set explicit dimensions:
```swift
Space()
    .width(16, .dp)
```
This behaves like a constant spacer and does not rely on weight.

## When to use Space

Use `Space` when:
- You need flexible spacing between views
- You want layout-driven gaps instead of hardcoded margins
- You want to distribute empty space predictably

Avoid `Space` when:
- Padding or margins are sufficient
- The layout does not support weights

`Space` is a simple but powerful tool for building clean, adaptive layouts while staying fully aligned with native Android behavior.