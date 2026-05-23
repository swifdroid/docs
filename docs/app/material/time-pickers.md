# Time pickers

Material time pickers choose a time of day with Material chrome (clock or input).

**Droid does not wrap Material time-picker APIs at the current source pin.** Classic [`TimePicker`](../classic/time-picker.md) is a placeable control without time APIs.

## Use it when…

Do not plan Material time-picker UI against Droid yet. Collect a time with text input and validate it in app code.

## How it behaves

There is no Material time-picker constructor, hour/minute API, or time-change listener at this pin.

## Notes

This page will grow only after a live Material time-picker wrapper lands in a commit pin with real methods.
