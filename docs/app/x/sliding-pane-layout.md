# SlidingPaneLayout

Use `SlidingPaneLayout` when a Swift view tree needs the AndroidX `SlidingPaneLayout` container with child views supplied inside it.

Droid lets you declare those children through a `@BodyBuilder` content initializer. This page covers construction of that view tree.

## Use it when…

Use the content initializer when you want to declare the container and its child views together in Swift.

## How it behaves

The Swift API provides two construction forms: an optional `Int32` ID alone, or an optional ID with a required `@BodyBuilder` closure. The closure can supply one or more child views. These signatures describe construction; they do not assign pane roles or specify how Android arranges or interacts with the children.

## Examples

The first root view contains a text child labeled “No item selected.” The second contains two text children labeled “Choose a record” and “Record details.”

```swift
let emptyStateContent = FrameLayout(id: 100)
emptyStateContent.body {
    SlidingPaneLayout(id: 200) {
        TextView(id: 201, "No item selected")
    }
}
```

```swift
let recordContent = FrameLayout(id: 110)
recordContent.body {
    SlidingPaneLayout(id: 210) {
        TextView(id: 211, "Choose a record")
        TextView(id: 212, "Record details")
    }
}
```

## Initializers

The ID is optional. To create the container without passing child content, use the ID-only initializer:

```swift
let layout = SlidingPaneLayout(id: 300)
```

## Notes

At the inspected source pin, the Droid type declares no `SlidingPaneLayout`-specific interaction controls.
