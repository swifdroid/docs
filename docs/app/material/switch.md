# Switch

Material switch is the Material-styled on/off control.

**Droid does not wrap `SwitchMaterial` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan Material switch chrome against Droid yet. Classic [`Switch`](../classic/switch.md) is a placeable control for settings rows.

## How it behaves

There is no `SwitchMaterial()` constructor to place and no Material checked-state API at this pin.

Android’s Material switch API (thumb/track, checked change listeners) must not be treated as available from Droid.

## Notes

This page will grow only after a live `SwitchMaterial` wrapper lands in a commit pin with real methods.
