# Magnifier

`Magnifier` shows a floating loupe over the screen so the user can see a zoomed copy of content under their finger. It is the same interaction Android uses when text becomes hard to read while selecting or dragging.

In Droid, `Magnifier` is a live object wrapper. It is not a view you drop into a layout builder. You receive an existing `android.widget.Magnifier` from Android, then show, move, zoom, and dismiss that loupe from Swift.

Because the loupe is a temporary overlay, treat it as interaction chrome rather than permanent UI. It appears near the finger, magnifies a source region, and disappears when the gesture ends.

## Use it when…

Use `Magnifier` when custom drag, selection, or precision controls need a platform-style zoom bubble over the touched content.

Skip it for ordinary zoom buttons or full-screen magnification. Those are better handled with layout scale, a dedicated zoom surface, or accessibility magnification.

## How it behaves

`show` places the loupe over a source point and optionally moves the magnified image to a center point. While it is visible, `update()` refreshes the captured content and position as the finger moves. `dismiss()` removes it.

`zoom` controls magnification. Geometry getters report the current source rectangle, loupe size, position, elevation, and default offsets Android uses when you do not override the center.

`content()` returns the magnified bitmap when Android has one. `originalContent()` returns the unzoomed capture. `overlay()` returns the drawable used as the loupe chrome.

`isClippingEnabled()` reports whether the source is clipped to the window bounds. `setOnOperationCompleteCallback` attaches an Android callback object for show/update/dismiss completion events.

## Examples

### Show a loupe during a drag

```swift
func onDragMove(_ magnifier: Magnifier, x: Float, y: Float) {
    magnifier.show(x, y)
    magnifier.update()
}
```

The loupe follows the finger and keeps the magnified region in sync.

### Pin the magnified image while the source moves

```swift
func onPrecisionDrag(_ magnifier: Magnifier, x: Float, y: Float) {
    magnifier.show(x, y, 240, 120)
}
```

Android samples the source near `(x, y)` and draws the zoomed image near the requested center.

### Finish the gesture cleanly

```swift
func onDragEnd(_ magnifier: Magnifier) {
    magnifier.dismiss()
}

func tuneZoom(_ magnifier: Magnifier) {
    magnifier.setZoom(2.5)
}
```

`dismiss()` ends the overlay. `setZoom(_:)` changes magnification before the next `show`.

## Initializers

```swift
Magnifier(androidObject)
```

The initializer accepts an existing JNI object for `android.widget.Magnifier`. Droid does not construct a magnifier from a view-builder block.

## Methods

### Show and dismiss

`show(_:_:)` opens the loupe at source coordinates. `show(_:_:_:_:)` also places the magnified image at a requested center. `update()` refreshes content and position. `dismiss()` closes the loupe.

```swift
magnifier.show(x, y)
magnifier.update()
magnifier.dismiss()
```

Call `update()` during a continuous gesture so the preview does not freeze.

### Zoom

`zoom()` reads the current magnification. `setZoom(_:)` changes it and returns the same magnifier for chaining.

```swift
let current = magnifier.zoom()
magnifier.setZoom(3.0)
```

### Geometry

`sourceWidth()`, `sourceHeight()`, and `sourcePosition()` describe the sampled region. `width()`, `height()`, and `position()` describe the loupe itself. `cornerRadius()` and `elevation()` report the chrome styling Android applies.

```swift
let source = magnifier.sourcePosition()
let loupeSize = magnifier.width()
```

`defaultHorizontalSourceToMagnifierOffset()` and `defaultVerticalSourceToMagnifierOffset()` return the offsets Android uses when the loupe center is not overridden.

### Content and overlay

`content()` returns the magnified bitmap when available. `originalContent()` returns the pre-zoom capture. `overlay()` returns the drawable used as loupe background.

```swift
if let bitmap = magnifier.content() {
    // inspect the magnified frame
}
```

`magnifierDefaultSize()` returns Android’s default loupe size. Both instance and class forms are available.

### Clipping and callbacks

`isClippingEnabled()` reports whether source sampling is clipped. `setOnOperationCompleteCallback(_:)` attaches a JNI-compatible callback object for operation completion.

```swift
magnifier.setOnOperationCompleteCallback(callback)
```

The callback must already be an Android-compatible listener object.

## Notes

The loupe is gesture chrome. Create or obtain the native `Magnifier` through the Android API that owns the interaction, then drive it with these methods for the lifetime of the gesture.
