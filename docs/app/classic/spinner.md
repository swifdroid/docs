# Spinner

`Spinner` is Android’s compact single-choice control that shows the current option and opens a popup or dialog for the rest.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; adapter, prompt, dropdown geometry, and item-click APIs are not wrapped yet.

## Use it when…

Use a spinner when the user picks one value from a short set and keeping every option permanently visible would take too much space.

For always-visible exclusive choices, use [`RadioGroup`](radio-group.md).

## How it behaves

`Spinner` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `adapter(_:)`, `prompt(_:)`, dropdown width/offsets, popup background, or item-click listeners. There is no Droid `SpinnerAdapter` type at this pin.

## Examples

### Place a country or option picker

```swift
Spinner(id: 5001)
    .padding(h: 16, v: 12)
```

### Disable the control while options load

```swift
Spinner(id: 5002)
    .enabled(false)
```

## Initializers

```swift
Spinner()
Spinner(id: 5001)
```

There is also a single-child content initializer:

```swift
Spinner(id: 5001) {
    // declarative child content via BodyBuilder
}
```

## Notes

Android’s `Spinner` is defined by its adapter and dropdown. None of that adapter surface is part of the current Droid API. Treat this page as placement plus shared view behavior only.
