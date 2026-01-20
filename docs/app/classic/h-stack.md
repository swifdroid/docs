# HStack

Horizontal container view used to arrange child views in a left-to-right order.

It is a thin, convenience wrapper around Android’s LinearLayout with its orientation fixed to `.horizontal`.  
Its purpose is to make horizontal layouts concise, readable, and familiar to Swift developers.

```swift
body {
    HStack {
        TextView("Some Text")
            .width(.wrapContent)
            .height(.wrapContent)
        Button("Tap Me")
            .width(.wrapContent)
            .height(48, .dp)
    }
    .width(.matchParent)
    .height(.wrapContent)
    .gravityForSubviews(.center)
    .padding(16, .dp)
}
```

All child views are laid out horizontally in the order they are declared.