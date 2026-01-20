# VStack

Vertical container view used to arrange child views in a top-to-bottom order.

It is a thin, convenience wrapper around Android’s LinearLayout with its orientation fixed to `.vertical`.  
Its purpose is to make vertical layouts concise, readable, and familiar to Swift developers.

```swift
body {
    VStack {
        TextView("Some Text")
            .width(.matchParent)
            .height(.wrapContent)
        Button("Tap Me")
            .width(.matchParent)
            .height(48, .dp)
    }
    .width(.matchParent)
    .height(.wrapContent)
    .gravityForSubviews(.center)
    .padding(16, .dp)
}
```

All child views are laid out vertically in the order they are declared.