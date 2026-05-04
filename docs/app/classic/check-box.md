# CheckBox

`CheckBox` is the classic Android control for an independent yes/no option.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; checked-state helpers and button drawable/tint configuration are not wrapped on this type.

## Use it when…

Use a checkbox for independent selections such as remembering a device, accepting terms, or enabling an optional feature.

For mutually exclusive choices, use [`RadioGroup`](radio-group.md). For a settings switch look, see [`Switch`](switch.md).

## How it behaves

`CheckBox` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

At this pin `CheckBox` inherits `View` only. It does **not** inherit [`CompoundButton`](compound-button.md) and does **not** conform to `Checkable`. Methods such as `checked(_:)`, `isChecked()`, `toggle()`, `buttonTintList(_:)`, and `buttonDrawable(_:)` are not available on `CheckBox`.

## Examples

### Place an options checkbox

```swift
CheckBox(id: 9001)
    .padding(h: 16, v: 12)
```

### React to a tap

```swift
let accept = CheckBox(id: 9002)

accept.onClick {
    // Handle activation. Checked-state reads/writes are not wrapped yet.
}
```

## Initializers

```swift
CheckBox()
CheckBox(id: 9001)
```

## Notes

Android’s `CheckBox` is a `CompoundButton`. The current Droid type is not. Use [`CompoundButton`](compound-button.md) when you need the verified `Checkable` methods (`checked(_:)`, `isChecked()`, `toggle()`).
