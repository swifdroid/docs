# MediaController

`MediaController` wraps Android's classic playback-controls UI. The current Droid wrapper inherits `FrameLayout` and combines queued controller configuration with live visibility and input operations.

## Use it when…

Use `MediaController` when a classic Android media player or `VideoView` needs Android's standard transport-control UI.

## How it behaves

These controller settings are queued Droid view properties:

- `anchorView(_:)`
- `setEnabled(_:)`
- `mediaPlayerControl(_:)` / `setMediaPlayer(_:)`
- `prevNextListeners(_:_:)`

`show()`, `show(_:)`, `hide()`, `isShowing()`, and input-event methods operate on the live Android instance.

## Example

```swift
let controller = MediaController()
    .anchorView(videoView)
    .setEnabled(true)
```

The configuration is queued until the controller has an Android instance.

## Initializers

```swift
MediaController()
MediaController(id: 1001)
```

The current wrapper also exposes a low-level initializer for an existing Android object plus an `ActivityContext` link.

## Anchor and enabled state

`anchorView(_:)` queues the view Android should use as the controller anchor.

`setEnabled(_:)` queues whether controller widgets are enabled.

## Media-player control

`mediaPlayerControl(_:)` and `setMediaPlayer(_:)` are aliases that queue a JNI-compatible Android `MediaController.MediaPlayerControl` object.

Droid exposes the bridge but does not construct that control object for you.

## Previous/next listeners

`prevNextListeners(_:_:)` queues JNI-compatible Android click-listener objects for the next and previous controls.

## Live visibility

`show()` shows the live controller indefinitely.

`show(_:)` shows it for a timeout in milliseconds.

`hide()` hides it.

`isShowing()` reports the current live visibility.

## Lower-level helpers

`makeControllerView()` returns the live controller hierarchy as `View?` when Android provides it.

The wrapper also forwards finish-inflate, key-dispatch, touch, and trackball helpers.

The current `MediaController` source declares `init(id:)` and the low-level existing-object initializer; it does not declare a dedicated declarative child-content initializer.
