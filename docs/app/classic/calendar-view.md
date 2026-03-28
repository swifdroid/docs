# CalendarView

`CalendarView` shows a full calendar for choosing and inspecting a date. It works well when the surrounding dates matter to the choice, such as booking, scheduling, or planning screens.

The selected date and date limits use milliseconds since January 1, 1970.

## Use it when…

Use `CalendarView` when the user should see a month-style calendar directly in the screen instead of opening a separate picker.

Because it occupies substantial vertical space, it is usually better for date-focused screens than for compact forms.

## How it behaves

Selection, range, week display, and appearance settings are builder-style properties. They can be configured before the Android view is realized.

Read methods such as `date()`, `minDate()`, `maxDate()`, and `shownWeekCount()` query the live Android instance. Keep a reference to the calendar when you need to inspect those values after the view is on screen.

The current date-change listener API accepts an Android/JNI-compatible listener object. There is no Swift closure convenience for date changes in the current surface.

## Examples

### Limit the selectable range

```swift
let earliest = 1_767_225_600_000
let latest = 1_769_817_600_000
let selected = 1_768_521_600_000

CalendarView()
    .minDate(earliest)
    .maxDate(latest)
    .date(selected)
```

The calendar opens with the selected date inside the configured range.

### Adjust the week display

```swift
CalendarView()
    .firstDayOfWeek(2)
    .shownWeekCount(5)
    .showWeekNumber(false)
```

This configures a five-week view, uses day value `2` as the first day of the week, and hides week numbers.

### Read the live selection

Once the view has been realized, read the currently selected date from the live Android instance:

```swift
func selectedDate(from calendar: CalendarView) -> Int {
    calendar.date()
}
```

## Initializers

Create a calendar with an optional stable id:

```swift
CalendarView()
CalendarView(id: 1001)
```

## Methods

### Select a date

`date()` reads the live selected date. `date(_:)` configures the selected date, while the animated overload also controls animation and centering.

```swift
let selected = calendar.date()

calendar.date(1_768_521_600_000)

calendar.date(
    1_768_521_600_000,
    animate: true,
    center: true
)
```

### Limit the available dates

`minDate()` and `maxDate()` read the live bounds. Their fluent counterparts configure the allowed range.

```swift
calendar
    .minDate(1_767_225_600_000)
    .maxDate(1_769_817_600_000)

let earliest = calendar.minDate()
let latest = calendar.maxDate()
```

### Choose the first day of the week

`firstDayOfWeek()` reads the live value. `firstDayOfWeek(_:)` configures the value used by Android.

```swift
calendar.firstDayOfWeek(2)

let firstDay = calendar.firstDayOfWeek()
```

### Control the visible week count

`shownWeekCount()` reads how many weeks are currently shown. `shownWeekCount(_:)` configures the count.

```swift
calendar.shownWeekCount(5)

let weeks = calendar.shownWeekCount()
```

### Show or hide week numbers

`showWeekNumber()` reads the live setting. `showWeekNumber(_:)` configures whether week numbers are displayed.

```swift
calendar.showWeekNumber(false)

let showsNumbers = calendar.showWeekNumber()
```

### Configure date appearance

The current surface exposes live appearance reads together with fluent setters for date text, weekday text, focused and unfocused month colors, selected-week color, week-number color, and separator color.

For example:

```swift
let primaryDateColor = 0x7f202020
let secondaryDateColor = 0x7f606060

calendar
    .dateTextAppearance(1001)
    .weekDayTextAppearance(1002)
    .focusedMonthDateColor(primaryDateColor)
    .unfocusedMonthDateColor(secondaryDateColor)
```

Use the corresponding zero-argument methods when you need to inspect the live values.

### Get bounds for a date

`boundsForDate(_:_:)` asks the live calendar to write the screen bounds for a date into an Android `Rect` object.

```swift
let found = calendar.boundsForDate(date, rect)
```

This is a lower-level API for code that already owns the required Android graphics object.
