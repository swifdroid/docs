# Radio button

Material radio button is the Material-styled exclusive-choice control.

**Droid does not wrap `MaterialRadioButton` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan Material radio chrome against Droid yet. Classic [`RadioButton`](../classic/radio-button.md) is a placeable control; use [`RadioGroup`](../classic/radio-group.md) when you need group selection APIs today.

## How it behaves

There is no `MaterialRadioButton()` constructor to place and no Material checked-state API at this pin.

Android’s Material radio API (button tint, checked change listeners) must not be treated as available from Droid.

## Notes

This page will grow only after a live `MaterialRadioButton` wrapper lands in a commit pin with real methods.
