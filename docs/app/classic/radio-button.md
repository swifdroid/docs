# RadioButton

`RadioButton` is one option in a mutually exclusive choice.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; checked state, button tint, and group coordination are not wrapped on this type.

## Use it when…

Use a radio button when the choice is one-of-many and the user should see the classic Android radio affordance.

For independent on/off options, see [`CheckBox`](check-box.md). For group selection APIs, see [`RadioGroup`](radio-group.md).

## How it behaves

`RadioButton` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

At this pin `RadioButton` inherits `View` only. It does **not** inherit [`CompoundButton`](compound-button.md) and does **not** conform to `Checkable`. Methods such as `checked(_:)`, `isChecked()`, `toggle()`, `buttonTintList(_:)`, and `buttonDrawable(_:)` are not available on `RadioButton`.

## Examples

### Place one option

```swift
RadioButton(id: 5001)
    .padding(h: 16, v: 12)
```

### React to a tap

```swift
let option = RadioButton(id: 5002)

option.onClick {
    // Handle activation. Checked-state reads/writes are not wrapped yet.
}
```

## Initializers

```swift
RadioButton()
RadioButton(id: 5001)
```

## Notes

Android’s `RadioButton` is a `CompoundButton`. The current Droid type is not. Use [`CompoundButton`](compound-button.md) when you need the verified `Checkable` methods (`checked(_:)`, `isChecked()`, `toggle()`), and [`RadioGroup`](radio-group.md) for group-level `check(_:)` / `clearCheck()` / `checkedRadioButtonId()`.
