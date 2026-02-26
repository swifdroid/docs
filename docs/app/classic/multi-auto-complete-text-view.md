# MultiAutoCompleteTextView

`MultiAutoCompleteTextView` wraps Android's token-oriented autocomplete field. It exposes tokenizer, filtering, validation, and replacement operations for inputs such as recipient lists or tags.

## Use it when…

Use it when the text is divided into multiple tokens and completion should operate on the token currently being edited.

## Current wrapper shape

In the current Droid source this type inherits directly from `View`, not from Droid's `AutoCompleteTextView` or `EditText` wrapper.

That means the current Swift surface does **not** automatically inherit Droid's `hint`, adapter, dropdown-configuration, or other `AutoCompleteTextView` convenience APIs.

Use only the methods explicitly exposed on `MultiAutoCompleteTextView` until that wrapper hierarchy expands.

## Examples

### Install a tokenizer

```swift
func makeTokenField<T: JObjectable>(tokenizer: T) -> MultiAutoCompleteTextView {
    MultiAutoCompleteTextView()
        .tokenizer(tokenizer)
}
```

`tokenizer(_:)` accepts a JNI-compatible Android tokenizer object; Droid does not construct one for you.

### Trigger validation on a live field

```swift
func validate(_ field: MultiAutoCompleteTextView) {
    field.performValidation()
}
```

## Initializers

```swift
MultiAutoCompleteTextView()
MultiAutoCompleteTextView(id: 1001)
```

## Methods

### Tokenizer

`tokenizer(_:)` is a queued Droid view property and configures Android's `MultiAutoCompleteTextView.Tokenizer` bridge.

### Filtering

`enoughToFilter()` reports whether the current token has enough content to filter.

`performFiltering(_:_:)` starts filtering for text and a key code. The four-argument overload additionally filters a specific text range.

These methods operate on the live Android instance.

### Validation and replacement

`performValidation()` asks Android to validate tokens using its configured behavior.

`replaceText(_:)` replaces the token currently being completed.

## Current limitation

The wrapper currently has no Droid-level adapter setter of its own and does not inherit the `AutoCompleteTextView` adapter/dropdown surface. A complete suggestion workflow therefore still requires lower-level integration outside the APIs shown on this page.
