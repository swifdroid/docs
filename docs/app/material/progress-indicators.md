# Progress indicators

Material progress indicators show determinate or indeterminate progress with Material motion (linear and circular).

**Droid does not wrap `LinearProgressIndicator` or `CircularProgressIndicator` at the current source pin.** The JNI names exist in the package namespace, but the Swift wrapper types are not live.

## Use it when…

Do not plan Material progress-indicator UI against Droid yet. Classic [`ProgressBar`](../classic/progress-bar.md) is a placeable control for layout purposes.

## How it behaves

There is no `LinearProgressIndicator()` / `CircularProgressIndicator()` constructor to place, no progress value API, and no indicator show/hide helpers at this pin.

Android’s Material indicator API (track thickness, indicator color, show/hide behavior) must not be treated as available from Droid.

## Notes

This page will grow only after live indicator wrappers land in a commit pin with real methods.
