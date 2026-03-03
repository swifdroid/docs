# TextClock

`TextClock` is Droid's self-updating clock label. The current wrapper inherits `TextView` and exposes 12-hour/24-hour formats, accessibility formats, time-zone selection, current-user time behavior, refresh control, and a clock-event delegate bridge.

## Use it when…

Use `TextClock` when Android should keep a displayed wall-clock time updated automatically, such as dashboards, status screens, or always-visible clock UI.

## How it behaves

Format setters, accessibility-format setters, `showCurrentUserTime(_:)`, `timeZone(_:)`, and `clockEventDelegate(_:)` are queued Droid view properties.

Format/time-zone getters, mode inspection, `refreshTime()`, and `disableClockTick()` operate on the live Android instance.

Because `TextClock` inherits `TextView`, normal text styling remains available.

## Examples

### Provide 12-hour and 24-hour formats

```swift
TextClock()
    .format12Hour("h:mm a")
    .format24Hour("HH:mm")
```

### Display a specific time zone

```swift
TextClock()
    .format24Hour("HH:mm")
    .timeZone("UTC")
```

## Initializers

```swift
TextClock()
TextClock(id: 1001)
```

## Methods

### Clock formats

`format12Hour(_:)` and `format24Hour(_:)` queue the patterns Android uses in the corresponding system mode.

`format12Hour()`, `format24Hour()`, and `format()` read current live values.

### Accessibility formats

`contentDescriptionFormat12Hour(_:)` and `contentDescriptionFormat24Hour(_:)` configure the patterns Android uses for the clock's content description.

### Time zone and current user

`timeZone(_:)` queues the time-zone ID used by the clock. `timeZone()` reads the current live value.

`showCurrentUserTime(_:)` configures whether the clock follows the current user's time.

`is24HourModeEnabled()` reports Android's current 24-hour mode.

### Refresh and ticking

`refreshTime()` asks the live clock to refresh its displayed time.

`disableClockTick()` stops automatic ticking updates on the live instance.

### Clock event delegate

`clockEventDelegate(_:)` accepts a JNI-compatible Android `TextClock.ClockEventDelegate` object.

Droid exposes the bridge but does not construct the delegate object for you.

### Lower-level helpers

Attach/detach, aggregated-visibility, and view-property encoding hooks are available for lower-level integrations.
