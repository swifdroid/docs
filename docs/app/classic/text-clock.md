# TextClock

`TextClock` is Android’s live clock text widget.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; format, time-zone, and 12/24-hour APIs are not wrapped yet.

## Use it when…

Use `TextClock` when the screen needs the classic Android clock control and format APIs can land later.

## How it behaves

`TextClock` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `format12Hour(_:)`, `format24Hour(_:)`, `timeZone(_:)`, or `is24HourMode()` at this pin.

## Examples

### Place a clock

```swift
TextClock(id: 2801)
    .padding(h: 16, v: 8)
```

## Initializers

```swift
TextClock()
TextClock(id: 2801)
```

## Notes

Android’s `TextClock` follows the system time and locale. Format controls are not part of the current Droid surface.
