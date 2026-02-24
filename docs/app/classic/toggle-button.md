# ToggleButton

`ToggleButton` is Android's classic button-shaped two-state control. Unlike the current `CheckBox`, `RadioButton`, and `Switch` wrappers, this Droid type currently inherits directly from `View` and exposes its own text/state properties.

Use it when the control itself should display different short labels for its two states.

## Use it when…

Use a toggle button when button-like presentation is intentional and the on/off labels fit naturally inside the control.

## How it behaves

`textOn(_:)`, `textOff(_:)`, `checked(_:)`, and `backgroundDrawable(_:)` are queued Droid view properties, so they can be configured while building the control.

The getter methods and lifecycle helpers read or call the live Android instance.

## Examples

### Build an ON/OFF toggle

```swift
ToggleButton()
    .textOn("ON")
    .textOff("OFF")
    .checked(true)
```

### Use domain-specific labels

```swift
ToggleButton()
    .textOn("Playing")
    .textOff("Paused")
```

## Initializers

```swift
ToggleButton()
ToggleButton(id: 1001)
```

## Methods

### State labels

`textOn(_:)` and `textOff(_:)` configure the two labels. `textOn()` and `textOff()` read the current strings from a live instance.

### Initial checked state

`checked(_:)` is a chainable queued property on `ToggleButton`:

```swift
ToggleButton().checked(false)
```

That is intentionally different from the live-only `CompoundButton.checked(_:)` API used by the current checkbox/radio/switch family.

### Background and live helpers

`backgroundDrawable(_:)` queues a custom background drawable.

`buttonStateDescription()` and `disabledAlpha()` read live Android values. `drawableStateChanged()` and `onFinishInflate()` forward the corresponding low-level lifecycle hooks.
