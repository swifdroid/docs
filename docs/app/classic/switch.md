# Switch

`Switch` is the classic Android two-state settings control. It inherits checked-state behavior from `CompoundButton` and adds switch-specific track sizing, padding, tint, drawable, icon, and text-style configuration.

Use it when changing the value should feel like immediately turning a setting on or off.

## Use it when…

Use a switch for persistent settings and modes where the visual on/off state should be obvious at a glance.

## How it behaves

`switchMinWidth(_:)`, `switchPadding(_:)`, `thumbTextPadding(_:)`, `trackDrawable(_:)`, `trackResource(_:)`, `trackTintList(_:)`, `switchTextAppearance(_:_:)`, `switchTypeface(...)`, and `trackIcon(_:)` are queued Droid view properties.

Checked-state methods inherited from `CompoundButton` operate on the live Android instance.

## Examples

### Configure spacing and track tint

```swift
func makeSwitch(tint: ColorStateList) -> Switch {
    Switch()
        .switchMinWidth(48)
        .switchPadding(8)
        .thumbTextPadding(4)
        .trackTintList(tint)
}
```

### Update a live switch

```swift
func enableSync(_ control: Switch) {
    control.checked(true)
}
```

## Initializers

```swift
Switch()
Switch(id: 1001)
```

## Methods

### Checked state

`Switch` inherits `isChecked()`, `checked(_:)`, `toggle()`, and `performClick()` from `CompoundButton`.

`checked(_:)` is a live operation, so configure the visual switch properties while building the view and update state after materialization.

### Layout and spacing

`switchMinWidth(_:)`, `switchPadding(_:)`, and `thumbTextPadding(_:)` accept dimensions in `.dp` by default.

The corresponding getter overloads read the live Android values and convert them into the requested `DimensionUnit`.

### Track appearance

`trackDrawable(_:)` installs a drawable, `trackResource(_:)` installs one by resource identifier, and `trackTintList(_:)` applies a tint.

`trackDrawable()` and `trackTintList()` read the current live values.

### Text appearance and typeface

`switchTextAppearance(_:_:)` forwards a context and style resource. `switchTypeface(_:)` and `switchTypeface(_:_:)` configure the typeface and optional style.

### Advanced track helpers

`setTrackResourceAsync(_:)`, `trackIcon(_:)`, and `setTrackIconAsync(_:)` expose Android's asynchronous/resource/icon track APIs for lower-level integrations.
