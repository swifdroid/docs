# MultiAutoCompleteTextView

`MultiAutoCompleteTextView` is Android’s multi-token completion field.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; tokenizers, adapters, and completion APIs are not wrapped yet.

## Use it when…

Use `MultiAutoCompleteTextView` when the field should complete multiple tokens and those APIs can land later.

For a single completion field, see [`AutoCompleteTextView`](auto-complete-text-view.md).

## How it behaves

`MultiAutoCompleteTextView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit [`EditText`](edit-text.md) at this pin and does not expose `adapter(_:)`, `tokenizer(_:)`, `setThreshold(_:)`, or dropdown helpers.

## Examples

### Place a multi-token field

```swift
MultiAutoCompleteTextView(id: 4001)
    .padding(h: 16, v: 12)
```

## Initializers

```swift
MultiAutoCompleteTextView()
MultiAutoCompleteTextView(id: 4001)
```

## Notes

Android’s `MultiAutoCompleteTextView` completes each token after a tokenizer boundary. That surface is not part of the current Droid API.
