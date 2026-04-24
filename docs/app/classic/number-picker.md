# NumberPicker

`NumberPicker` is Android’s wheel control for choosing a value from a bounded range or a short label list.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; min/max/value, wrapping, displayed labels, and listeners are not wrapped yet.

## Use it when…

Use a number picker when the user should choose one value from a small, obvious range and a wheel feels faster than typing.

For free numeric input, use [`EditText`](edit-text.md). For one-of-few named options, a radio group or spinner-shaped control is usually clearer.

## How it behaves

`NumberPicker` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `minValue(_:)`, `maxValue(_:)`, `value(_:)`, `wrapSelectorWheel(_:)`, `displayedValues(_:)`, formatters, or value-change listeners at this pin.

## Examples

### Place a quantity control

```swift
NumberPicker(id: 4001)
    .padding(h: 16, v: 12)
```

### Disable it until a range is known

```swift
NumberPicker(id: 4002)
    .enabled(false)
```

## Initializers

```swift
NumberPicker()
NumberPicker(id: 4001)
```

## Notes

Android’s `NumberPicker` owns range, wrapping, and displayed labels. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
