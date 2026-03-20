# VideoView

`VideoView` is Droid's wrapper for Android's classic video playback widget. The current surface exposes live source selection, playback control, seeking, playback-state reads, media-controller attachment, subtitle hooks, and JNI-compatible media-player listeners.

## Use it when…

Use `VideoView` when Android's classic built-in video playback behavior is sufficient for your screen.

## How it behaves

The current `VideoView` wrapper does not expose queued playback properties.

Source assignment, controller attachment, listeners, playback actions, and state reads operate on the live Android instance.

## Example

```swift
func startVideo(_ video: VideoView) {
    video.setVideoPath("/sdcard/Movies/demo.mp4")
    video.start()
}
```

The `VideoView` must already have a live Android instance for these calls to take effect.

## Initializers

```swift
VideoView()
VideoView(id: 1001)
```

## Video source

`setVideoPath(_:)` sets a path on the live view.

`setVideoURI(_:)` sets an optional Android `Uri`.

The overload `setVideoURI(_:_:)` also accepts a JNI-compatible Android headers map.

## Playback

Live actions include:

- `start()`
- `pause()`
- `resume()`
- `suspend()`
- `stopPlayback()`
- `seekTo(_:)`

Live reads include:

- `isPlaying()`
- `currentPosition()`
- `duration()`
- `bufferPercentage()`
- `audioSessionId()`
- `canPause()`
- `canSeekBackward()`
- `canSeekForward()`

## Media controller

`setMediaController(_:)` attaches an optional live `MediaController`.

This is an immediate live call, not a queued builder property in the current wrapper.

## Listener bridges

`setOnCompletionListener(_:)`, `setOnErrorListener(_:)`, `setOnInfoListener(_:)`, and `setOnPreparedListener(_:)` accept JNI-compatible Android media-player listener objects.

Droid exposes these listener bridges but does not construct the listener objects for you.

## Subtitle and lower-level helpers

`addSubtitleSource(_:_:)` and `setSubtitleWidget(_:)` expose Android subtitle hooks.

The wrapper also forwards drawing, measurement, layout, attach/detach, key, touch, trackball, and adjusted-size helpers.
