# MediaController

`MediaController` is Android’s classic media playback control panel.

In the current Droid source pin it is a placeable view control with an additional existing-object initializer. Shared view interaction and layout APIs work; player attachment, show/hide, and transport controls are not wrapped yet.

## Use it when…

Use `MediaController` when the screen needs the classic Android media controller name and player APIs can land later.

## How it behaves

`MediaController` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `setMediaPlayer(_:)`, `setAnchorView(_:)`, `show()`, `hide()`, or transport button wiring at this pin.

## Examples

### Place a media controller

```swift
MediaController(id: 3501)
    .padding(h: 0, v: 0)
```

## Initializers

```swift
MediaController()
MediaController(id: 3501)
MediaController(id: 3501, existingObject) { activityContext }
```

The third form wraps an already-created Android `MediaController` object and a context link.

## Notes

Android’s `MediaController` drives a `MediaPlayer` / `VideoView`. That binding surface is not part of the current Droid API.
