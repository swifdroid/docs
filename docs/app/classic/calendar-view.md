# CalendarView

`CalendarView` shows a month-style calendar for choosing or inspecting a date when surrounding days matter to the decision.

In the current Droid source pin it is a placeable view control with a single-child content initializer. Shared view interaction and layout APIs work; selection, min/max bounds, week display, and date-change listeners are not wrapped yet.

## Use it when…

Use `CalendarView` when the user should see a full month grid in the screen instead of opening a separate picker, such as booking or scheduling layouts.

Because it takes substantial vertical space, prefer a compact picker for short forms.

## How it behaves

`CalendarView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type inherits `FrameLayout` at this pin and accepts declarative child content. It does **not** expose `minDate(_:)`, `maxDate(_:)`, `date()`, `shownWeekCount(_)`, `firstDayOfWeek(_)`, or date-change listeners.

## Examples

### Place a calendar

```swift
CalendarView(id: 1101)
    .padding(h: 8, v: 8)
```

### Add a caption under the calendar

```swift
CalendarView(id: 1101) {
    TextView("Selected date")
}
```

## Initializers

```swift
CalendarView()
CalendarView(id: 1101)

CalendarView(id: 1101) {
    // single child content via BodyBuilder
}
```

## Notes

Android’s `CalendarView` owns selection and range behavior. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
