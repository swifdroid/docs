# AutoCompleteTextView

`AutoCompleteTextView` is Android’s text field with completion suggestions.

In the current Droid source pin it inherits [`EditText`](edit-text.md). Text editing from that family works. Completion adapters, thresholds, and suggestion APIs are not wrapped yet.

## Use it when…

Use `AutoCompleteTextView` when the field should grow completion behavior and the adapter surface can land later.

For plain text input, use [`EditText`](edit-text.md).

## How it behaves

`AutoCompleteTextView` keeps the current [`EditText`](edit-text.md) / [`TextView`](text-view.md) text surface, including `text(_:)`, plus shared view configuration such as `onClick(_:)`, padding, `enabled(_:)`, and `visibility(_:)`.

The current type does **not** expose `adapter(_:)`, `setThreshold(_:)`, `showDropDown()`, `dismissDropDown()`, or item-click listeners at this pin.

## Examples

### Place a completion field

```swift
AutoCompleteTextView(id: 3901)
    .hint("City")
    .padding(h: 16, v: 12)
```

### Start from a known value

```swift
AutoCompleteTextView(id: 3902, "Berlin")
```

## Initializers

```swift
AutoCompleteTextView()
AutoCompleteTextView(id: 3901)
AutoCompleteTextView(id: 3901, someStringState)
```

## Notes

Android’s `AutoCompleteTextView` is defined by its adapter and threshold. That surface is not part of the current Droid API.
