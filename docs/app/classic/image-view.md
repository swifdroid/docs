# ImageView

`ImageView` displays drawable, resource, URI, or icon content. The current Droid wrapper exposes queued image assignment, tinting, bounds constraints, and a small set of live drawable helpers.

## Use it when…

Use `ImageView` for avatars, icons, thumbnails, illustrations, and other image content that belongs in the view hierarchy.

## How it behaves

These setters are queued Droid view properties:

- `adjustViewBounds(_:)`
- `imageDrawable(_:)`
- `imageResource(_:)`
- `imageURI(_:)`
- `imageIcon(_:)`
- `imageTintList(_:)`
- `maxWidth(_:)`
- `maxHeight(_:)`

Matching getters and drawable-maintenance helpers operate on the live Android instance.

## Examples

### Configure a resource image before mounting

```swift
ImageView()
    .imageResource(1001)
    .adjustViewBounds()
    .maxWidth(320)
    .maxHeight(320)
```

### Configure a drawable

```swift
let drawable: Drawable? = nil

ImageView()
    .imageDrawable(drawable)
```

## Initializers

```swift
ImageView()
ImageView(id: 1001)
```

## Image sources

`imageDrawable(_:)` queues an optional Android `Drawable`.

`imageResource(_:)` queues a drawable resource ID.

`imageURI(_:)` queues an optional Android `Uri`.

`imageIcon(_:)` queues a JNI-compatible Android `Icon` object.

## Bounds and tint

`adjustViewBounds(_:)` queues whether Android should adjust the view bounds to preserve the drawable aspect ratio.

`maxWidth(_:)` and `maxHeight(_:)` queue maximum dimensions.

`imageTintList(_:)` queues an optional `ColorStateList`.

The corresponding getters read live Android state.

## Live drawable helpers

`drawable()` returns the current live `Drawable?`.

`invalidateDrawable(_:)`, `jumpDrawablesToCurrentState()`, and `verifyDrawable(_:)` forward directly to the mounted Android view.

`hasOverlappingRendering()` reports Android's current rendering characteristic.

## Async Android helpers

`setImageIconAsync(_:)`, `setImageResourceAsync(_:)`, and `setImageURIAsync(_:)` call the live Android APIs and return Android's `Runnable` object as `JObject?` when available.

The wrapper defines an `ImageView.ScaleType` enum, but the current source does not expose a scale-type setter/getter through this wrapper.
