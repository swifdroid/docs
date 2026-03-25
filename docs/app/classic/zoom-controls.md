# ZoomControls

`ZoomControls` wraps Android's legacy paired zoom-in/zoom-out controls. It combines queued button configuration and listener bridges with live show/hide/focus behavior.

## Use it when…

Use `ZoomControls` when an existing Android-style screen needs the classic paired +/− zoom controls.

For new interfaces, gesture-driven zoom plus custom controls is usually more flexible.

## How it behaves

Queued Droid view properties:

- `zoomInEnabled(_:)` / `isZoomInEnabled(_:)`
- `zoomOutEnabled(_:)` / `isZoomOutEnabled(_:)`
- `zoomSpeed(_:)`
- `onZoomInClickListener(_:)`
- `onZoomOutClickListener(_:)`

`show()`, `hide()`, `hasFocus()`, and `onTouchEvent(_:)` operate on the live Android instance.

## Example

```swift
ZoomControls()
    .zoomInEnabled(true)
    .zoomOutEnabled(true)
    .zoomSpeed(250)
```

The enabled/speed configuration can be queued before the Android view exists.

## Initializers

```swift
ZoomControls()
ZoomControls(id: 1001)
```

## Enabling each control

`zoomInEnabled(_:)` controls the zoom-in button.

`zoomOutEnabled(_:)` controls the zoom-out button.

The `isZoomInEnabled(_:)` and `isZoomOutEnabled(_:)` methods are setter aliases in the current wrapper; they are not Boolean getters.

## Showing and hiding

`show()` and `hide()` are live actions.

`hasFocus()` reads the current live focus state.

## Click-listener bridges

`onZoomInClickListener(_:)` and `onZoomOutClickListener(_:)` queue JNI-compatible Android `View.OnClickListener` objects.

Droid exposes these bridges but does not construct the listener objects for you.

## Touch handling

`onTouchEvent(_:)` forwards a live Android `MotionEvent` and returns Android's handled/not-handled result.
