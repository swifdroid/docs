# VideoView

`VideoView` is Android’s classic built-in video playback widget.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; source assignment, playback control, seeking, and media-controller hooks are not wrapped yet.

## Use it when…

Use `VideoView` when Android’s classic playback widget is enough and you do not need a custom player surface yet.

## How it behaves

`VideoView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `setVideoPath(_:)`, `start()`, `pause()`, `seekTo(_:)`, `isPlaying()`, `setMediaController(_:)`, or player listener bridges at this pin.

## Examples

### Place a video surface

```swift
VideoView(id: 1201)
    .padding(h: 0, v: 0)
```

### Keep playback UI hidden until the source is ready

```swift
let video = VideoView(id: 1202)
    .visibility(.gone)

// later
video.visibility(.visible)
```

## Initializers

```swift
VideoView()
VideoView(id: 1201)
```

## Notes

Android’s `VideoView` owns path/URI assignment and playback commands. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
