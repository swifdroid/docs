# ImageButton

`ImageButton` is the image-only button control.

In the current Droid source pin it inherits [`ImageView`](image-view.md) and adds no extra wrapped methods. Shared view interaction and layout APIs work; image sources and button chrome are not wrapped yet.

## Use it when…

Use `ImageButton` when an image is the visual surface of an action, such as a toolbar icon button.

For display-only image content, use [`ImageView`](image-view.md).

## How it behaves

`ImageButton` behaves like the current [`ImageView`](image-view.md) shell: placeable in the hierarchy, with `onClick(_:)`, padding, `enabled(_:)`, and `visibility(_:)`.

Image assignment and tinting are not part of the current Droid surface.

## Examples

### Place an icon action

```swift
ImageButton(id: 1901)
    .onClick {
        // handle the action
    }
```

## Initializers

```swift
ImageButton()
ImageButton(id: 1901)
```

## Notes

There is no image-source API on this type at the current pin. Wire content when the image wrappers grow, then document the real methods here.
