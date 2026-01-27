# EditText

`EditText` is a text field for values the user can enter and modify. It is a good fit for form fields, search queries, settings, and other places where text should move between the UI and application state.

Passing a string state gives the field two-way synchronization: the current state becomes the field text, later state changes update the field, and user edits are written back to that state.

## Use it when…

Use `EditText` when typing is part of the interaction. It is appropriate for names, email addresses, search terms, notes, and other editable strings.

If the value is only being displayed, use [`TextView`](text-view.md) instead.

## How it behaves

`EditText` inherits the text configuration available on `TextView`, including useful setters such as `hint(_:)`. With `EditText($value)`, Droid installs text-change handling in addition to the normal state-backed text update, which is what makes the binding two-way.

Selection methods such as `selectAll()` and `setSelection(index:)` are imperative: they call the existing Android view instance. Invoke them after the field has been created, for example from a button or another user event, rather than chaining them onto a newly constructed field before it is on screen.

## Examples

### Bind a field to state

```swift
@State var email = ""

body {
    VStack {
        TextView("Email")

        EditText($email)
            .hint("name@example.com")

        TextView($email)
    }
    .padding(16)
}
```

Typing in the field changes `email`, and the second label reflects the same state. Changing `email` elsewhere also updates the field.

### Select the current value from an action

```swift
@State var query = "Swift"
let searchField = EditText()

body {
    VStack {
        searchField
            .text($query)
            .hint("Search")

        Button("Select all")
            .onClick {
                searchField.selectAll()
            }
    }
    .padding(16)
}
```

The selection call runs after the field exists, so it can act on the underlying Android instance.

## Initializers

Create an empty field or bind it directly to a string state:

```swift
EditText()
EditText($email)
EditText(id: 1001)
```

## Methods

### Select all text

`selectAll()` selects the field's current text. Because it operates on the existing view instance, call it after the field has been built.

```swift
Button("Select all")
    .onClick {
        searchField.selectAll()
    }
```

### Move or extend the selection

`setSelection(index:)` moves the caret to an offset. The range overload selects between two offsets, while `extendSelection(index:)` extends the current selection.

```swift
Button("Move cursor")
    .onClick {
        searchField.setSelection(index: 0)
    }
```

For a range:

```swift
Button("Select first word")
    .onClick {
        searchField.setSelection(start: 0, end: 5)
    }
```

### Style shortcuts

`setStyleShortcutsEnabled(_:)` enables or disables supported keyboard style shortcuts on the existing field instance.

```swift
Button("Enable style shortcuts")
    .onClick {
        searchField.setStyleShortcutsEnabled()
    }
```

Use it after the field has been created for the same reason as the selection methods.

### Observe text changes

The inherited `textChangedListener(beforeTextChanged:onTextChanged:afterTextChanged:)` API is useful when you need a callback in addition to state binding.

```swift
EditText()
    .textChangedListener(
        beforeTextChanged: nil,
        onTextChanged: { event in
            Log.i("Current input: \(event.p0 ?? "")")
        },
        afterTextChanged: nil
    )
```
