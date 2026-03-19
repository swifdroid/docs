# ImageButton

`ImageButton` is Droid's Android image-button wrapper. It inherits the full `ImageView` image surface and adds only a small amount of image-button-specific low-level behavior.

## Use it when…

Use `ImageButton` when an image is also the visual surface of an action.

For display-only image content, use `ImageView`.

## How it behaves

Image assignment, tinting, maximum dimensions, and adjust-view-bounds behavior are inherited from `ImageView` and keep the same queued semantics.

The two methods declared directly by the current `ImageButton` wrapper operate on the live Android instance.

## Example

```swift
ImageButton()
    .imageResource(1001)
    .adjustViewBounds()
    .maxWidth(48)
    .maxHeight(48)
```

## Initializers

```swift
ImageButton()
ImageButton(id: 1001)
```

## Inherited image configuration

Because `ImageButton` inherits `ImageView`, it can use:

- `imageDrawable(_:)`
- `imageResource(_:)`
- `imageURI(_:)`
- `imageIcon(_:)`
- `imageTintList(_:)`
- `adjustViewBounds(_:)`
- `maxWidth(_:)`
- `maxHeight(_:)`

The matching live getters and drawable helpers are inherited too.

## ImageButton-specific helpers

`onSetAlpha(_:)` forwards Android's alpha callback and returns Android's Boolean result.

`onResolvePointerIcon(_:_:)` forwards pointer-icon resolution for a live motion event and returns the Android pointer icon as `JObject?` when available.

General view interaction behavior comes from the inherited Droid view hierarchy; this wrapper does not add a separate image-button click API.
