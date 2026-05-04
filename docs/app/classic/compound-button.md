# CompoundButton

`CompoundButton` is the shared Android base for controls with a checked/unchecked state. In the current Droid source pin it is the type that actually exposes the `Checkable` methods.

Use the concrete control that best matches the interaction when one exists. Reach for `CompoundButton` itself when you need the verified checked-state API on a live instance.

## Use it when…

Use this surface when you need to read, set, or toggle checked state on a live checkable control.

Button drawable/tint configuration and checked-change listeners are not wrapped yet, so this page documents the checked-state methods only.

## How it behaves

`CompoundButton` inherits [`Button`](button.md) and conforms to `Checkable`. Text configuration from the `TextView` family and shared view interaction from `View` remain available through those parents.

`checked(_:)`, `isChecked()`, and `toggle()` operate on the live Android instance. `checked(_:)` is a live operation and does not return the control for chaining.

## Examples

### Change live checked state

```swift
func enable(_ control: CompoundButton) {
    control.checked(true)
}
```

### Read the current state

```swift
if !control.isChecked() {
    control.checked(true)
}
```

### Flip a setting

```swift
control.toggle()
```

## Initializers

```swift
CompoundButton()
CompoundButton(id: 1001)
```

## Methods

### Checked state

`isChecked()` reads the live state. `checked(_:)` sets it, and `toggle()` flips it.

```swift
let on = control.isChecked()
control.checked(!on)
```

These three methods come from the `Checkable` protocol and are the verified checked-state surface at this pin.

## Notes

At this pin [`CheckBox`](check-box.md) does **not** inherit `CompoundButton` and does **not** receive these methods. `buttonDrawable(_:)`, `buttonTintList(_:)`, `onCheckedChangeListener(_:)`, and `performClick()` are not part of the current Droid `CompoundButton` API.
