# Toolbar

The platform Android toolbar is currently exposed by Droid as `Toolbar1`.

At the current source pin `Toolbar1` is a placeable view control. Shared view interaction and layout APIs work; title/subtitle, navigation and overflow icons, menu inflation, content insets, and toolbar listeners are not wrapped yet.

## Use it when…

Use `Toolbar1` when you intentionally want Android’s platform `android.widget.Toolbar` and only need to place it in the hierarchy for now.

Apps built around AppCompat or Material components will usually prefer their corresponding toolbar wrappers once those surfaces match your screen.

## How it behaves

`Toolbar1` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type is **not** a `ViewGroup`. It does not expose `title(_:)`, `subtitle(_:)`, `navigationIcon(_:)`, `logo(_:)`, `inflateMenu(_:)`, overflow helpers, content-inset setters, or menu-click listeners at this pin.

## Examples

### Place a platform toolbar

```swift
Toolbar1(id: 7001)
    .padding(h: 0, v: 0)
```

### Keep it visible only on detail screens

```swift
let bar = Toolbar1(id: 7002)
    .visibility(.gone)

// when showing detail
bar.visibility(.visible)
```

## Initializers

```swift
Toolbar1()
Toolbar1(id: 7001)
```

There is no content initializer at this pin.

## Notes

The Swift type name is `Toolbar1` while the JNI class name remains `android.widget.Toolbar`. Earlier drafts described a full `ViewGroup` toolbar API that is not in the pinned source. Treat this page as placement plus shared view behavior only.
