# DrawerLayout

`DrawerLayout` is available in Droid as an AndroidX container. The Swift API documented here supports construction, with or without child views.

## Use it when…

Use this page when you need to construct the available AndroidX `DrawerLayout` `ViewGroup`. At the verified source version, the Swift wrapper documents construction only.

## How it behaves

You can create the container without body content or provide children through a `BodyBuilder` closure. The builder accepts multiple children, despite its closure type being named `BodyBuilder.SingleView`.

## Examples

Construct a container with an explicit ID and no body content:

```swift
let drawerLayout = DrawerLayout(id: 100)
```

Provide multiple ordinary child views through the builder:

```swift
DrawerLayout {
    TextView("Main content")
    TextView("Additional content")
}
```

Both snippets demonstrate construction only. They do not assign a drawer role or side, open a drawer, or establish Android runtime behavior.

## Initializers

`init(id: Int32? = nil)` constructs the container without body content. The optional ID defaults to `nil`.

`init(id: Int32? = nil, @BodyBuilder content: BodyBuilder.SingleView)` constructs the container with builder content. The ID has the same default; the content closure is required and can provide multiple children. The builder and its closure run in main-actor context.

## Notes

At the verified source version, the Swift wrapper does not expose supported drawer-role or placement configuration, open/close calls, lock-mode controls, state callbacks/listeners, or a verified drawer interaction API.
