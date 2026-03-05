# TimePicker

`TimePicker` is Droid's classic Android time-of-day selector. The current wrapper exposes hour/minute configuration, matching live getters, compatibility aliases for current hour/minute, 12-hour/24-hour presentation, and picker-mode inspection.

## Use it when…

Use `TimePicker` when users should choose a valid time of day without manually parsing text or AM/PM input.

## How it behaves

`hour(_:)`, `minute(_:)`, the `currentHour(_:)` / `currentMinute(_:)` aliases, and `is24HourView(_:)` are queued Droid view properties.

Hour/minute and mode getters read the live Android instance.

## Examples

### Configure a 24-hour time

```swift
TimePicker()
    .hour(9)
    .minute(30)
    .is24HourView(true)
```

### Configure a 12-hour picker

```swift
TimePicker()
    .hour(7)
    .minute(45)
    .is24HourView(false)
```

## Initializers

```swift
TimePicker()
TimePicker(id: 1001)
```

## Methods

### Hour and minute

`hour(_:)` and `minute(_:)` queue the selected time.

`hour()` and `minute()` read the current live selection.

### Current-hour/current-minute aliases

`currentHour(_:)` forwards to `hour(_:)`, and `currentMinute(_:)` forwards to `minute(_:)`.

The getter aliases `currentHour()` and `currentMinute()` return the same live values as `hour()` and `minute()`.

### 12-hour and 24-hour display

`is24HourView(_:)` queues whether Android should display the picker in 24-hour mode or with AM/PM controls.

### Picker mode

`mode()` reads the current Android picker mode.

## Current limitation

The current Droid `TimePicker` wrapper does not expose an on-time-changed listener bridge. This page therefore does not assume a callback API that is not present in the current wrapper.