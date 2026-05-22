# Side sheets

Material side sheets slide in from the edge for supplementary content on large screens.

**Droid does not wrap Material side-sheet APIs at the current source pin.** There is no live side-sheet Swift type to place or drive.

## Use it when…

Do not plan Material side-sheet UI against Droid yet. A classic side panel you show/hide with `visibility(_:)` is the current option.

## How it behaves

There is no side-sheet constructor, edge-grabbing API, or sheet-state callback at this pin.

## Notes

This page will grow only after a live side-sheet wrapper lands in a commit pin with real methods.
