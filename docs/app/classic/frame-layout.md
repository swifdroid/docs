# FrameLayout

`FrameLayout` stacks children in the same area, making it useful when one piece of content needs overlays such as a loading indicator, badge, or centered action.

Later children are drawn above earlier children. Child layout params such as `gravityInParent(_:)` and margins control where an overlay sits inside the frame.

## Use it when…

Use `FrameLayout` when the screen has a primary layer with a small number of views placed over it.

When many views need relationships to one another rather than simple overlay placement, use a layout designed for those relationships instead.

## How it behaves

Children share the frame's bounds. Their order determines their visual stacking order: the first child forms the lower layer and later children appear above it.

Use `gravityInParent(_:)` on a child to place it within the frame. Margins can offset that placement from the selected edge or corner.

## Examples

### Show a loading indicator over content

```swift
FrameLayout {
    ScrollView {
        TextView("Content goes here…")
            .padding(16)
    }

    ProgressBar()
        .gravityInParent(.center)
}
.width(.matchParent)
.height(.matchParent)
```

The scrollable content remains the lower layer while the progress indicator is centered above it.

### Put a badge in the top-right corner

```swift
FrameLayout {
    TextView("Inbox")
        .padding(16)

    TextView("3")
        .padding(h: 8, v: 4)
        .textColor(.black)
        .backgroundColor(.lightGray)
        .gravityInParent([.top, .end])
        .marginTop(12)
        .marginEnd(12)
}
```

The badge is drawn above the Inbox label and offset from the top-end corner.

### Center an action above a background layer

```swift
FrameLayout {
    View()
        .backgroundColor(.lightGray)

    Button("Get started")
        .gravityInParent(.center)
}
.height(220)
```

The background fills the lower layer and the button is positioned at the center of the frame.

## Initializers

Create an empty frame or provide its children directly:

```swift
FrameLayout()

FrameLayout {
    TextView("Child")
}
```

## Layout params

Apply `gravityInParent(_:)` to a child when it needs a specific position within the frame.

```swift
FrameLayout {
    ProgressBar()
        .gravityInParent(.center)
}
```
