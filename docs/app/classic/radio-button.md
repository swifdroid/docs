# RadioButton

`RadioButton` represents one option in a mutually exclusive choice. In the current Droid source it inherits the checkable state and button styling surface from `CompoundButton`.

Use several radio buttons when the user must choose exactly one item from a small set. Group coordination is a separate concern from the individual `RadioButton` wrapper.

## Use it when…

Use a radio button when the choice is one-of-many rather than an independent on/off preference.

## How it behaves

The wrapper inherits live checked-state operations and queued button drawable/tint properties from `CompoundButton`.

`RadioButton` also exposes Android's low-level `onInitializeAccessibilityNodeInfo(_:)` hook.

## Examples

### Style a radio button

```swift
func makeRadioButton(tint: ColorStateList) -> RadioButton {
    RadioButton()
        .buttonTintList(tint)
}
```

### Select a live radio button

```swift
radioButton.checked(true)
```

Check the current value with:

```swift
let selected = radioButton.isChecked()
```

## Initializers

```swift
RadioButton()
RadioButton(id: 1001)
```

## Inherited checkable API

`RadioButton` inherits `isChecked()`, `checked(_:)`, `toggle()`, `performClick()`, button drawable/tint configuration, and listener support from `CompoundButton`.

`checked(_:)` works on the live Android instance and is not a chainable builder modifier.

## Accessibility hook

`onInitializeAccessibilityNodeInfo(_:)` forwards an Android accessibility node object to the live view. Most application code should let Android invoke this as part of its normal accessibility lifecycle.
