# Lists

Material lists present stacked rows with icons, text, and trailing controls.

**Droid does not wrap Material list APIs at the current source pin.** Classic [`ListView`](../classic/list-view.md) is a placeable control without adapter APIs.

## Use it when…

Do not plan Material list-row UI against Droid yet. Compose rows from [`TextView`](../classic/text-view.md) and containers you lay out yourself.

## How it behaves

There is no Material list-item constructor, two-line list binding, or list adapter bridge at this pin.

## Notes

This page will grow only after live Material list wrappers land in a commit pin with real methods.
