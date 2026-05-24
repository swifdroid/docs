# Extended FABs

Extended FABs are wider Material floating actions with a label next to the icon.

**Droid does not wrap `ExtendedFloatingActionButton` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan extended-FAB UI against Droid yet. [`FloatingActionButton`](fabs.md) is the pin-backed FAB today.

## How it behaves

There is no `ExtendedFloatingActionButton()` constructor, expand/collapse API, or extended-FAB listener at this pin.

Do not use `FloatingActionButton.expanded(_:)` as if it were a full extended-FAB API — that flag exists on the regular FAB only.

## Notes

This page will grow only after a live `ExtendedFloatingActionButton` wrapper lands in a commit pin with real methods.
