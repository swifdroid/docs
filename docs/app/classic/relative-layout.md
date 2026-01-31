# RelativeLayout

`RelativeLayout` positions children using rules relative to the parent or to other sibling views, such as placing one view below another or aligning a child with a parent edge.

Sibling-relative rules use view ids as anchors, which makes this layout useful when a small number of relationships can be expressed directly.

## Use it when…

Use `RelativeLayout` when the layout has a small number of views and their relationships are easy to describe with parent and sibling rules.

For larger screens with many interdependent relationships, a constraint-based layout is usually easier to maintain.

## How it behaves

Give an anchor view an explicit `id`, then refer to that id from sibling layout params such as `below(_:)`, `toEndOf(_:)`, or `alignBaseline(_:)`.

Parent-relative helpers such as `alignParentStart()`, `alignParentBottom()`, `centerHorizontal()`, and `centerVertical()` do not need an anchor id.

## Examples

### Place a subtitle below a title

```swift
RelativeLayout {
    TextView(id: 1, "Title")
        .alignParentTop()
        .alignParentStart()

    TextView("Subtitle")
        .below(1)
        .alignStart(1)
}
.padding(16)
```

The subtitle is positioned below the title and aligned to the title's start edge.

### Pin an action to the bottom center

```swift
RelativeLayout {
    Button("Continue")
        .alignParentBottom()
        .centerHorizontal()
        .marginBottom(16)
}
.width(.matchParent)
.height(220)
```

The button stays aligned with the parent's bottom edge and horizontal center.

### Align an action with a labeled row

```swift
RelativeLayout {
    TextView(id: 1, "Username")
        .alignParentStart()
        .centerVertical()

    Button("Edit")
        .toEndOf(1)
        .alignBaseline(1)
        .marginStart(12)
}
.padding(16)
```

The Edit button is placed after the Username label and shares its text baseline.

## Initializers

Create an empty layout or provide its children directly:

```swift
RelativeLayout()

RelativeLayout {
    TextView("Child")
}
```

## Layout params

Use parent-relative helpers when placement depends only on the container:

```swift
Button("Continue")
    .alignParentBottom()
    .centerHorizontal()
```

Use an explicit id when another child is the anchor:

```swift
TextView(id: 1, "Title")

TextView("Subtitle")
    .below(1)
    .alignStart(1)
```
