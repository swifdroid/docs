# Sliders

Material sliders pick a value or range by dragging a thumb along a track (volume, price range, intensity).

**Droid does not wrap `Slider` or `RangeSlider` at the current source pin.** The JNI names exist in the package namespace, but the Swift wrapper types are not live.

## Use it when…

Do not plan Material slider UI against Droid yet. Classic [`SeekBar`](../classic/seek-bar.md) exists as a placeable control, but its progress APIs are not wired at this pin either.

## How it behaves

There is no `Slider()` / `RangeSlider()` constructor to place, no value/range API, and no slider listener bridge at this pin.

Android’s Material slider API (steps, labels, halo, tick marks) must not be treated as available from Droid.

## Notes

This page will grow only after live `Slider` / `RangeSlider` wrappers land in a commit pin with real methods.
