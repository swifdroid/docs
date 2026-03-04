# DatePicker

`DatePicker` is Droid's classic Android date-selection control. The current wrapper inherits `FrameLayout` and exposes selected date reads, minimum/maximum bounds, calendar/spinner visibility, first-day-of-week configuration, date updates, listener bridges, state restoration, and autofill helpers.

## Use it when…

Use `DatePicker` when users should choose a valid calendar date without manually typing and parsing date text.

## How it behaves

Date bounds, calendar/spinner visibility, first-day-of-week, enabled state, date-change listener, and validation callback are queued Droid view properties.

Selected date getters and actions such as `updateDate(...)` operate on the live Android instance.

## Examples

### Configure the picker presentation

```swift
DatePicker()
    .calendarViewShown(true)
    .spinnersShown(false)
    .setEnabled(true)
```

### Read the selected date from a live picker

```swift
let picker = DatePicker()

let year = picker.year()
let month = picker.month()
let day = picker.dayOfMonth()
```

The getter methods read Android's current live selection.

## Initializers

```swift
DatePicker()
DatePicker(id: 1001)
```

The wrapper also exposes a `FrameLayout`-style content initializer for declarative child content.

## Methods

### Selected date

`year()`, `month()`, and `dayOfMonth()` read the currently selected date.

`updateDate(year:month:dayOfMonth:)` updates the live picker.

The low-level `init(_:_:_:_:)` bridge initializes a live picker with a date and an optional Android date-change listener object.

### Date bounds

`minDate(_:)` and `maxDate(_:)` queue Android date bounds in milliseconds since January 1, 1970.

`minDate()` and `maxDate()` read the current live values.

### Calendar and spinner presentation

`calendarViewShown(_:)` and `spinnersShown(_:)` configure which picker UI Android shows.

Their getter counterparts read the current live state.

`calendarView()` returns the underlying `CalendarView?` when available.

### Week and mode

`firstDayOfWeek(_:)` configures Android's first day of the week, while `firstDayOfWeek()` reads the live value.

`mode()` reads the current picker mode.

### Enabled state

`setEnabled(_:)` queues the enabled state. `isEnabled()` reads the current live state.

### Date-change and validation callbacks

`onDateChangedListener(_:)` and `validationCallback(_:)` accept JNI-compatible Android objects and queue them for the picker.

Droid exposes these bridges but does not construct the listener or validation callback objects for you.

### State and autofill

The wrapper exposes save/restore-state, accessibility, configuration-change, and autofill helpers for lower-level integrations.