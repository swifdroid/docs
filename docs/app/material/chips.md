# Chips

Material chips are compact elements for filters, choices, and actions (topics, attributes, “selected” tags).

**Droid does not wrap `Chip` / `ChipGroup` at the current source pin.** The JNI names exist in the package namespace, but the Swift wrapper types are not live yet.

## Use it when…

Do not plan chip UI against Droid yet. Use a row of [`MaterialButton`](buttons/button.md) controls or a [`RadioGroup`](../classic/radio-group.md) when you need a similar interaction today.

## How it behaves

There is no `Chip()` or `ChipGroup()` constructor to place, no selection API, and no chip listener bridge at this pin.

Android’s Material chip API (single-choice chips, filter chips, entry chips, chip groups) must not be treated as available from Droid.

## Notes

This page will grow only after live `Chip` / `ChipGroup` wrappers land in a commit pin with real methods.
