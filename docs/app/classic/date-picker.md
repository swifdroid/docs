# DatePicker

`DatePicker` is Android’s calendar-date selection control.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; selected-date reads, min/max bounds, `updateDate`, calendar/spinner presentation, and date-change listeners are not wrapped yet.

## Use it when…

Use a date picker when the user should choose a valid calendar date instead of typing and parsing date text.

For a compact list of dates or a custom calendar UI, build the selection surface yourself and keep the validated date in app state.

## How it behaves

`DatePicker` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `FrameLayout` and does **not** expose `updateDate(...)`, `minDate(_:)`, `maxDate(_:)`, `calendarViewShown(_:)`, `spinnersShown(_:)`, `year()` / `month()` / `dayOfMonth()`, or listener bridges at this pin.

## Examples

### Place a date control

```swift
DatePicker(id: 6001)
    .padding(h: 16, v: 12)
```

### Hide it until the user opts in

```swift
let picker = DatePicker(id: 6002)
    .visibility(.gone)

// later
picker.visibility(.visible)
```

## Initializers

```swift
DatePicker()
DatePicker(id: 6001)
```

## Notes

Android’s `DatePicker` owns the selected date, bounds, and spinner/calendar chrome. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
