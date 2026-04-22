# Switch

`Switch` is the classic Android two-state control for settings that should look immediately on or off.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work as on any other control; switch-specific checked-state and track/thumb styling are not wrapped yet.

## Use it when…

Use a switch for a persistent setting or mode where the on/off meaning should be obvious at a glance, such as enabling notifications or dark theme.

Do not use it when the user is choosing among several exclusive options — that is radio-group territory — or when a compact checkbox fits the form better.

## How it behaves

`Switch` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and toggle `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `CompoundButton` and does **not** conform to `Checkable`. Methods such as `checked(_:)`, `isChecked()`, `toggle()`, `switchMinWidth(_:)`, `trackTintList(_:)`, and similar Android switch APIs are not available on this Droid type at the current pin.

## Examples

### Place a settings switch

```swift
Switch(id: 2001)
    .padding(h: 16, v: 12)
```

The control is created and can sit in a settings row like any other view.

### React to a tap

```swift
let syncSwitch = Switch(id: 2001)

syncSwitch.onClick {
    // Handle the user activating the control.
    // Checked-state reads/writes are not wrapped yet.
}
```

### Disable the control until setup finishes

```swift
Switch(id: 2002)
    .enabled(false)
```

## Initializers

```swift
Switch()
Switch(id: 2001)
```

## Notes

Android’s full `Switch` widget has checked state, thumb/track drawables, and text appearance. None of that is part of the current Droid `Switch` surface. Treat this page as placement plus shared view behavior only.
