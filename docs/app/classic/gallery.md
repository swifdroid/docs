# Gallery

`Gallery` is a legacy Android widget for horizontally browsing items.

Android deprecated the widget long ago. Keep it for maintenance of existing Gallery-based screens; for new UI, prefer a modern list or scrolling layout.

## Use it when…

Use `Gallery` only when an existing Android screen already depends on it and replacing the widget is outside the current change.

## How it behaves

Droid currently supports constructing the Gallery and applying inherited view configuration such as size.

Keep usage to that stable surface. Lower-level gesture, spacing, and layout-management controls are intentionally outside this page.

## Example

Create the legacy container and size it like any other view:

```swift
Gallery()
    .width(.matchParent)
    .height(220)
```

## Initializers

Create a Gallery with an optional stable id:

```swift
Gallery()
Gallery(id: 1001)
```

## Notes

Treat `Gallery` as compatibility UI. New browsing interfaces are usually better served by `RecyclerView` or another modern layout that is designed for current Android patterns.
