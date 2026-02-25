# AutoCompleteTextView

`AutoCompleteTextView` is Droid's classic single-value autocomplete field. It inherits `EditText` behavior and adds dropdown, filtering, adapter, completion, validator, and listener APIs.

Use it when text input should be assisted by suggestions from a compatible Android adapter.

## Use it when…

Use autocomplete for values such as cities, contacts, tags, commands, or catalog entries where users still type text but benefit from suggestions.

## How it behaves

Configuration such as `threshold(_:)`, dropdown size/offset/background, completion hint, IME mode, adapter, validator, and listener setters is queued as Droid view properties.

Runtime methods such as `showDropDown()`, `dismissDropDown()`, `isPopupShowing()`, `performCompletion()`, `performValidation()`, and the getter methods operate on the live Android instance.

The current initializer is `AutoCompleteTextView(id:)`. There is no `@State`-binding initializer on this wrapper snapshot.

## Examples

### Configure filtering and dropdown presentation

```swift
AutoCompleteTextView()
    .threshold(2)
    .completionHint("Suggestions")
    .dropDownWidth(280)
    .dropDownVerticalOffset(4)
```

### Attach a compatible suggestion adapter

```swift
func makeAutoComplete<T: JObjectable>(adapter: T) -> AutoCompleteTextView {
    AutoCompleteTextView()
        .threshold(1)
        .setAdapter(adapter)
}
```

`setAdapter(_:)` accepts a JNI-compatible object. Droid does not construct the suggestion adapter for you.

## Initializers

```swift
AutoCompleteTextView()
AutoCompleteTextView(id: 1001)
```

## Methods

### Filtering threshold

`threshold(_:)` configures how many characters are required before filtering. `threshold()` reads the live Android value.

```swift
AutoCompleteTextView().threshold(3)
```

`enoughToFilter()` reports whether the current text has reached that threshold.

### Dropdown configuration

The wrapper exposes queued configuration for:

- `dropDownAlwaysVisible(_:)`
- `dropDownDismissedOnCompletion(_:)`
- `dropDownAnchor(_:)`
- `dropDownAnimationStyle(_:)`
- `dropDownBackgroundDrawable(_:)` / `dropDownBackgroundResource(_:)`
- `dropDownHeight(_:)` / `dropDownWidth(_:)`
- horizontal and vertical offsets
- `completionHint(_:)`
- `inputMethodMode(_:)`

Corresponding getter methods read current live values.

### Showing and completing

`showDropDown()` and `showDropDownAfterLayout()` display the popup from a live field. `dismissDropDown()` closes it.

`performCompletion()` completes the currently selected suggestion. `refreshAutoCompleteResults()` asks Android to refresh the result set.

### Adapter, validator, and listeners

`setAdapter(_:)`, `validator(_:)`, `onDismissListener(_:)`, `setOnItemClickListener(_:)`, `setOnItemSelectedListener(_:)`, and `setOnClickListener(_:)` accept JNI-compatible objects and queue them for the Android instance.

The wrapper exposes these bridges, but it does not create the adapter/listener/validator objects for you.

### Low-level filtering helpers

`performFiltering`, `replaceText`, `setText(_:_:)`, key-event methods, focus/lifecycle hooks, and completion callbacks are exposed for lower-level integrations. Most app code can stay on the higher-level dropdown and adapter APIs.