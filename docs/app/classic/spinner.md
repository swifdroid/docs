# Spinner

`Spinner` is Android's compact single-choice popup control. The current Droid wrapper exposes adapter assignment, prompt text, dropdown geometry/background, gravity, enabled state, click helpers, and item-click listener bridges.

## Use it when…

Use a spinner when users choose one value from a relatively short set and keeping all options visible would take too much screen space.

## How it behaves

`adapter(_:)`, `prompt(_:)`, dropdown offsets/width, gravity, popup background, enabled state, and item-click listeners are queued Droid view properties.

Getter methods and popup/click/state helpers operate on the live Android instance.

## Examples

### Configure prompt and popup width

```swift
Spinner()
    .prompt("Country")
    .dropDownWidth(240)
    .dropDownVerticalOffset(4)
```

### Attach a compatible spinner adapter

```swift
func makeSpinner<T: SpinnerAdapter & JObjectable>(adapter: T) -> Spinner {
    Spinner()
        .adapter(adapter)
        .prompt("Choose an option")
}
```

The adapter must conform to Droid's `SpinnerAdapter` bridge and be JNI-compatible.

## Initializers

```swift
Spinner()
Spinner(id: 1001)
```

## Methods

### Adapter

`adapter(_:)` queues the adapter used by Android to provide spinner items.

The current wrapper does not expose a separate high-level `selection(_:)` setter, so this page does not assume one.

### Prompt and dropdown

`prompt(_:)` sets popup/dialog prompt text and `promptId(_:)` sets it from a resource.

`dropDownWidth(_:)`, `dropDownHorizontalOffset(_:)`, `dropDownVerticalOffset(_:)`, `gravity(_:)`, `popupBackgroundDrawable(_:)`, and `popupBackgroundResource(_:)` configure presentation.

Corresponding getter methods read current live values where exposed.

### Popup and click helpers

`isPopupShowing()` reports whether the popup is visible. `performClick()` asks Android to open/activate the spinner interaction.

`onItemClickListener(_:)` and `setOnItemClickListenerInt(_:)` accept JNI-compatible item-click listener objects.

### Low-level lifecycle

Layout/measurement, saved-state, touch, pointer-icon, dialog-click, and accessibility hooks are exposed for lower-level integration.
