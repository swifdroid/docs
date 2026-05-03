# ImageSwitcher

`ImageSwitcher` is Android’s two-view control for switching between images.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; image setters, factory bridges, and switcher animations are not wrapped yet.

## Use it when…

Use `ImageSwitcher` when the screen needs a dedicated control that will later switch images with Android’s built-in two-view behavior.

For a single image slot, use [`ImageView`](image-view.md).

## How it behaves

`ImageSwitcher` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `ViewSwitcher` / `ViewAnimator` APIs. It does not expose `imageDrawable(_:)`, `imageResource(_:)`, `imageURI(_:)`, `factory(_:)`, or animation helpers at this pin.

## Examples

### Place an image switcher

```swift
ImageSwitcher(id: 2001)
    .padding(h: 8, v: 8)
```

## Initializers

```swift
ImageSwitcher()
ImageSwitcher(id: 2001)
```

## Notes

Android’s `ImageSwitcher` depends on factory callbacks and image sources. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
