# ToggleButton

`ToggleButton` is Android’s two-state button that shows on/off labels.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; checked state and text-on/text-off labels are not wrapped yet.

## Use it when…

Use `ToggleButton` when the control should look like a labeled button that stays pressed in one of two states.

For a settings switch look, see [`Switch`](switch.md). For an independent checkbox, see [`CheckBox`](check-box.md).

## How it behaves

`ToggleButton` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** conform to `Checkable` and does not expose `checked(_:)`, `isChecked()`, `toggle()`, `textOn(_:)`, or `textOff(_:)` at this pin.

## Examples

### Place a toggle button

```swift
ToggleButton(id: 3301)
    .padding(h: 16, v: 12)
```

## Initializers

```swift
ToggleButton()
ToggleButton(id: 3301)
```

## Notes

Android’s `ToggleButton` is a `CompoundButton` with on/off text. That checked-state and label surface is not part of the current Droid API. Use [`CompoundButton`](compound-button.md) when you need verified `Checkable` methods.
