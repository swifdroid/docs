# CheckBox

`CheckBox` is the classic Android control for an independent yes/no option. The current Droid type inherits the full `CompoundButton` checked-state, drawable, tint, and listener surface.

Use it for preferences and forms where changing one option does not automatically change the others.

## Use it when…

Use a checkbox for independent selections such as remembering a device, subscribing to updates, or enabling an optional feature.

## How it behaves

`CheckBox` itself is a thin concrete wrapper over `CompoundButton`. Button drawable/tint configuration can be queued while building the view, while checked-state reads and writes use the live Android instance.

## Examples

### Build a tinted checkbox

```swift
func makeCheckBox(tint: ColorStateList) -> CheckBox {
    CheckBox()
        .buttonTintList(tint)
}
```

### Update a live checkbox

```swift
func markAccepted(_ checkBox: CheckBox) {
    checkBox.checked(true)
}
```

Read the current state with:

```swift
let accepted = checkBox.isChecked()
```

## Initializers

```swift
CheckBox()
CheckBox(id: 1001)
```

## Inherited checkable API

`CheckBox` inherits `isChecked()`, `checked(_:)`, `toggle()`, `performClick()`, button drawable/tint configuration, button-state accessibility helpers, and the low-level checked-change listener property from `CompoundButton`.

`checked(_:)` is a live operation rather than a chainable builder modifier.
