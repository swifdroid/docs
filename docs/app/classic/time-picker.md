# TimePicker

`TimePicker` is Android’s time-of-day selection control.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; hour/minute configuration, 12/24-hour presentation, and mode inspection are not wrapped yet.

## Use it when…

Use a time picker when the user should choose a valid time of day instead of typing free-form time text.

For duration input or a custom clock face, build that surface yourself and keep the validated time in app state.

## How it behaves

`TimePicker` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `hour(_:)`, `minute(_:)`, `currentHour(_:)` / `currentMinute(_:)`, `is24HourView(_:)`, or `mode()` at this pin.

## Examples

### Place a time control

```swift
TimePicker(id: 8001)
    .padding(h: 16, v: 12)
```

### Disable it until a schedule exists

```swift
TimePicker(id: 8002)
    .enabled(false)
```

## Initializers

```swift
TimePicker()
TimePicker(id: 8001)
```

## Notes

Android’s `TimePicker` owns hour/minute and AM/PM or 24-hour presentation. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
