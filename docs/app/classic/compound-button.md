# CompoundButton

`CompoundButton` is the shared Android base for controls with a checked/unchecked state, including `CheckBox`, `RadioButton`, and `Switch`.

Use the concrete control that best matches the interaction. `CompoundButton` is mainly useful for understanding the state, drawable, tint, and listener behavior those controls inherit.

## Use it when…

Use this API surface when you need to read or change checked state on a live checkable control, customize its button drawable or tint, or attach an Android checked-change listener.

## How it behaves

`buttonDrawable(_:)`, `buttonTintList(_:)`, `buttonIcon(_:)`, and `onCheckedChangeListener(_:)` are Droid view properties. They can be configured while the control is being built and are applied when an Android instance is available.

`isChecked()`, `checked(_:)`, `toggle()`, `performClick()`, and the getter methods operate on the live Android instance.

## Examples

### Configure the button tint

```swift
func makeCheckBox(tint: ColorStateList) -> CheckBox {
    CheckBox()
        .buttonTintList(tint)
}
```

### Change a live checked state

```swift
func enable(_ checkBox: CheckBox) {
    checkBox.checked(true)
}
```

`checked(_:)` is a live operation and does not return the control for chaining.

## Initializers

```swift
CompoundButton()
CompoundButton(id: 1001)
```

## Methods

### Checked state

`isChecked()` reads the live state. `checked(_:)` sets it, and `toggle()` flips it.

```swift
if !control.isChecked() {
    control.checked(true)
}
```

`performClick()` asks Android to perform the click behavior and returns whether it was handled.

### Button drawable and tint

`buttonDrawable(_:)` accepts either a `Drawable?` or a drawable resource identifier. `buttonTintList(_:)` applies a `ColorStateList` tint.

```swift
CheckBox()
    .buttonDrawable(drawableResourceId)
```

`buttonDrawable()` and `buttonTintList()` read the current values from a live instance.

### Accessibility and advanced drawable helpers

`buttonStateDescription()` reads Android's current accessibility description for the checked state. `defaultStateDescription()` restores Android's default state-description behavior.

`setButtonDrawableAsync(_:)`, `buttonIcon(_:)`, `setButtonIconAsync(_:)`, and `onResolveDrawables(_:)` are available for lower-level integration.

### Checked-change listener

`onCheckedChangeListener(_:)` accepts a JNI-compatible listener object and queues it like other Droid view properties. The wrapper does not construct that listener for you.
