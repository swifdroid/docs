# HorizontalScrollView

`HorizontalScrollView` is Android’s classic horizontal scrolling container.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; child content, scroll commands, and scroll-state callbacks are not wrapped yet.

## Use it when…

Use `HorizontalScrollView` when a row of content should scroll sideways and you need the classic Android control name in the hierarchy.

## How it behaves

`HorizontalScrollView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

Unlike [`ScrollView`](scroll-view.md) at this pin, it does **not** offer a content initializer and is not a `ViewGroup` subclass in the current Droid type. Horizontal scroll commands and smooth scrolling are not wrapped.

## Examples

### Place a horizontal scroller

```swift
HorizontalScrollView(id: 2401)
    .padding(h: 0, v: 8)
```

## Initializers

```swift
HorizontalScrollView()
HorizontalScrollView(id: 2401)
```

## Notes

Android’s `HorizontalScrollView` owns horizontal scrolling of its child. That child-content and scroll API is not part of the current Droid surface.
