# Checkbox

Material checkbox is the Material-styled independent yes/no control.

**Droid does not wrap `MaterialCheckBox` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan Material checkbox chrome against Droid yet. Classic [`CheckBox`](../classic/check-box.md) is a placeable control; checked-state helpers are not wrapped there either.

## How it behaves

There is no `MaterialCheckBox()` constructor to place and no Material checked-state API at this pin.

Android’s Material checkbox API (button tint, checked change listeners) must not be treated as available from Droid. When you need verified `Checkable` methods today, use [`CompoundButton`](../classic/compound-button.md).

## Notes

This page will grow only after a live `MaterialCheckBox` wrapper lands in a commit pin with real methods.
