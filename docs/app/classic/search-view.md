# SearchView

`SearchView` is Android’s dedicated search control: an expandable field for typing a query and submitting it.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; query, hint, iconify, suggestion, and searchable-info APIs are not wrapped yet.

## Use it when…

Use `SearchView` when search is a first-class action on the screen and you want the familiar expand/collapse search chrome.

Do not use it as a generic text field. For free-form input, use [`EditText`](edit-text.md).

## How it behaves

`SearchView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `LinearLayout`. It does not expose `query(_:)`, `queryHint(_:)`, `iconified(_:)`, `submitButtonEnabled(_:)`, suggestion adapters, or listener bridges at this pin.

## Examples

### Place a search entry point

```swift
SearchView(id: 3001)
    .padding(h: 12, v: 8)
```

The control is created and can sit in an app bar or search row.

### Keep it hidden until the user opens search

```swift
let search = SearchView(id: 3002)
    .visibility(.gone)

// later
search.visibility(.visible)
```

## Initializers

```swift
SearchView()
SearchView(id: 3001)
```

## Notes

Android’s `SearchView` can own query text, submit behavior, and suggestions. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
