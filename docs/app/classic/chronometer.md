# Chronometer

`Chronometer` is Android’s elapsed-time text widget.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; base time, format, started state, and tick listeners are not wrapped yet.

## Use it when…

Use `Chronometer` when the screen should show a classic running timer and the timer APIs can land later.

## How it behaves

`Chronometer` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit [`TextView`](text-view.md) at this pin and does not expose `setBase(_:)`, `start()`, `stop()`, `setFormat(_:)`, or tick listeners.

## Examples

### Place a timer slot

```swift
Chronometer(id: 2701)
    .padding(h: 16, v: 8)
```

## Initializers

```swift
Chronometer()
Chronometer(id: 2701)
```

## Notes

Android’s `Chronometer` counts from a base `SystemClock` value. Start/stop and format APIs are not part of the current Droid surface.
