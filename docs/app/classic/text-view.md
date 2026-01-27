# TextView

`TextView` displays text that the user reads but does not edit. It works well for titles, labels, status messages, helper copy, and values that change while a screen is open.

Pass a plain `String` for fixed text or a string state when the label should stay synchronized with application state. A state-backed `TextView` applies the current value when the view is created and updates the Android view when that state changes.

## Use it when…

Use `TextView` whenever text is output rather than input: headings, descriptions, counters, validation messages, connection status, or values derived from state.

When the user needs to type or edit the value, use [`EditText`](edit-text.md) instead.

## How it behaves

Text configuration is applied to the underlying text view when the view is built. Methods such as `textSize(_:_:)`, `maxLines(_:)`, and `gravity(_:)` let the text participate naturally in the surrounding layout without turning the page into a list of Android properties.

When text comes from state, Droid keeps listening to that state after the view is created. Changing the state updates the displayed text without requiring you to replace the `TextView` yourself.

## Examples

### Show a title and supporting text

```swift
body {
    VStack {
        TextView("Account")
            .textSize(24)

        TextView("Manage the details used to sign in.")
            .textSize(14)
            .maxLines(2)
    }
    .padding(16)
}
```

The title is visually distinct while the supporting copy is allowed to wrap to at most two lines.

### Keep a status label in sync with state

```swift
@State var status = "Not connected"

body {
    VStack {
        TextView($status)

        Button("Connect")
            .onClick {
                status = "Connected"
            }
    }
    .padding(16)
}
```

The label receives the current state value when it is built. Tapping the button changes the state, and the displayed text follows that change.

## Initializers

Create an empty view, pass fixed text directly, or bind a string state:

```swift
TextView()
TextView("Hello")
TextView($status)
TextView(id: 1001, "Title")
```

## Methods

### Text and state

`text(_:)` changes the displayed text and accepts either a plain string value or a compatible string state.

```swift
TextView()
    .text("Ready")
```

A state can be attached the same way when the value should keep updating:

```swift
TextView()
    .text($status)
```

### Text size

`textSize(_:_:)` sets the text size. The default unit is `.sp`.

```swift
TextView("Headline")
    .textSize(22)
```

### Line limits

`maxLines(_:)` limits the maximum number of displayed lines and is useful when surrounding layout height should stay predictable.

```swift
TextView("A longer message that may wrap when space is narrow.")
    .maxLines(2)
```

### Gravity

`gravity(_:)` controls the alignment of text inside the view when its bounds have extra room.

```swift
TextView("Centered")
    .width(.matchParent)
    .gravity(.center)
```

### Text changes

`textChangedListener(beforeTextChanged:onTextChanged:afterTextChanged:)` registers callbacks for text changes. It is especially useful with editable subclasses such as `EditText`.

```swift
EditText()
    .textChangedListener(
        beforeTextChanged: nil,
        onTextChanged: { event in
            Log.i("Text changed to: \(event.p0 ?? "")")
        },
        afterTextChanged: nil
    )
```

## Notes

Some `TextView` operations are imperative rather than deferred. For example, `append(_:)` talks to the existing Android instance, so use it only after the view has been created. For ordinary declarative text changes, prefer `text(_:)` or state-backed text.
