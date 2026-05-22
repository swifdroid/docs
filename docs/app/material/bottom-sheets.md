# Bottom sheets

Material bottom sheets slide up from the bottom for modal or persistent supplementary content.

**Droid does not wrap Material bottom-sheet APIs at the current source pin.** There is no live `BottomSheet*` Swift type to place or drive.

## Use it when…

Do not plan Material bottom-sheet UI against Droid yet. Use a classic container you show/hide with `visibility(_:)` when you need a temporary surface today.

## How it behaves

There is no bottom-sheet constructor, peek-height API, drag callbacks, or modal state API at this pin.

Android’s Material bottom-sheet behavior (coordinator-dependent dragging, scrim) must not be treated as available from Droid.

## Notes

This page will grow only after a live bottom-sheet wrapper lands in a commit pin with real methods.
