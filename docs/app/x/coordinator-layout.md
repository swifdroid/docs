# CoordinatorLayout

`CoordinatorLayout` configures relationships between children, such as an app bar and its scrolling sibling or a child that references another view as its anchor.

## Use it when…

Use it when an `AppBarLayout` and scrolling content need the app-bar relationship provided by `AppBarLayout.ScrollingViewBehavior`. Both views belong directly inside the `CoordinatorLayout`, with the behavior attached to the scrolling sibling.

Use it when a child needs to reference another child's ID as an anchor and supply anchor gravity.

## How it behaves

`CoordinatorLayout` is a `ViewGroup`. Configure child relationships through `View` layout-param methods called on its direct children. For example, `.behavior(...)` assigns a behavior to that child, while `.anchorId(...)` supplies the ID of its anchor.

The container's body builder can contain multiple children. Each child's layout params describe that child's relationship within the container.

## Examples

Place these expressions in a main-actor UI body.

### App bar and scrolling content

This configures the app-bar/scrolling-sibling relationship, with enough content to exercise scrolling inside the bounded container. The scroll flags belong to the `TextView` inside `AppBarLayout`; the scrolling sibling receives `ScrollingViewBehavior`.

```swift
CoordinatorLayout {
    AppBarLayout {
        TextView("Inbox")
            .width(.matchParent)
            .height(56)
            .scrollFlags(.scroll, .enterAlways)
    }
    .width(.matchParent)
    .height(.wrapContent)

    NestedScrollView {
        TextView(String(repeating: "Message\n", count: 100))
            .width(.matchParent)
            .height(.wrapContent)
    }
    .width(.matchParent)
    .height(.matchParent)
    .behavior(AppBarLayout.ScrollingViewBehavior())
}
.width(.matchParent)
.height(320)
```

### Reference an anchor

The second child references the first child's ID, `10`, and supplies combined bottom/end anchor gravity.

```swift
CoordinatorLayout {
    TextView(id: 10, "Header")
        .width(160)
        .height(56)

    TextView("Related content")
        .width(.wrapContent)
        .height(.wrapContent)
        .anchorId(10)
        .anchorGravity([.bottom, .end])
}
.width(.matchParent)
.height(160)
```

## Initializers

`init(id: Int32? = nil)` creates a container without builder content. The optional ID defaults to `nil`.

```swift
CoordinatorLayout()
CoordinatorLayout(id: 20)
```

`init(id: Int32? = nil, @BodyBuilder content: BodyBuilder.SingleView)` accepts a content builder and the same optional ID. Despite the `SingleView` name, the builder can contain multiple children, as in the examples above.

```swift
CoordinatorLayout(id: 20) {
    TextView("First")
    TextView("Second")
}
```

## Layout params

These six public `View` methods configure layout params on direct children of a `CoordinatorLayout`. Each takes an explicit value and returns the same view for chaining; the return value can also be discarded. The small snippets below show parameter calls to use on children inside the container.

### `behavior(_:)`

Assigns a `Behavior` to the child. For the app-bar relationship, attach the concrete `AppBarLayout.ScrollingViewBehavior` to the scrolling sibling shown above.

```swift
TextView("List").behavior(AppBarLayout.ScrollingViewBehavior())
```

### `anchorId(_:)`

Sets the child's anchor reference using an `Int32` view ID. Give the referenced view a matching ID, as in the anchor example.

```swift
TextView("Related").anchorId(10)
```

### `anchorGravity(_:)`

Supplies the child's gravity relative to its anchor. `Gravity` is an `OptionSet`: `[.bottom, .end]` constructs one combined `Gravity` value passed to this method.

```swift
TextView("Related").anchorGravity([.bottom, .end])
```

### `dodgeInsetEdges(_:)`

Sets the child's `dodgeInsetEdges` parameter, which selects edges for inset dodging. Its current argument type is `Gravity`.

```swift
TextView("Inset").dodgeInsetEdges(.bottom)
```

### `insetEdge(_:)`

Sets the child's `insetEdge` parameter to the supplied edge value. Its current argument type is also `Gravity`.

```swift
TextView("Top inset").insetEdge(.top)
```

### `keyline(_:)`

Sets the child's keyline parameter using a raw `Int32` value. This call supplies the parameter; it does not define keylines on the container.

```swift
TextView("Marker").keyline(0)
```

## Limitations

`Behavior` is a global, non-generic Swift class with a public no-argument initializer, `Behavior()`. It maps to the Java inner class `CoordinatorLayout.Behavior`. The Swift API does not expose user-implementable scroll, drag, or swipe callbacks. Use an exposed concrete behavior such as `AppBarLayout.ScrollingViewBehavior` for the relationship it supports.

`dodgeInsetEdges` and `insetEdge` currently accept `Gravity`; a dedicated edge-flags type remains a source TODO. `keyline` accepts raw `Int32`, with a position type also left as a TODO. These parameter calls alone do not specify keyline units or guarantee a particular layout result.
