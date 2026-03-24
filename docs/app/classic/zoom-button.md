# ZoomButton

`ZoomButton` wraps Android's legacy single zoom button. The current Droid wrapper provides queued enabled/speed configuration plus a few live input-event helpers.

## Use it when…

Use `ZoomButton` when a legacy Android screen needs a dedicated zoom control.

For new interfaces, gesture-based zoom or a custom control is usually more flexible.

## How it behaves

These settings are queued Droid view properties:

- `setEnabled(_:)`
- `zoomSpeed(_:)`

Input-event helpers such as `onKeyUp(_:_:)`, `onLongClick(_:)`, `onTouchEvent(_:)`, and `dispatchUnhandledMove(_:_:)` operate on the live Android instance.

## Example

```swift
ZoomButton()
    .setEnabled(true)
    .zoomSpeed(250)
```

The configuration can be applied before the Android view instance exists.

## Initializers

```swift
ZoomButton()
ZoomButton(id: 1001)
```

## Enabled state

`setEnabled(_:)` queues Android's enabled state. Calling it without an argument enables the button.

## Repeat speed

`zoomSpeed(_:)` queues the delay, in milliseconds, between repeated callbacks while the button is held.

## Live event helpers

`dispatchUnhandledMove(_:_:)` forwards Android focus navigation.

`onKeyUp(_:_:)` forwards a live key-up event.

`onLongClick(_:)` and `onTouchEvent(_:)` forward Android's live interaction callbacks.

These helpers expose Android behavior directly; they do not create a higher-level zoom model for your content.
