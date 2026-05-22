# Menus

Material menus show a list of actions anchored to a trigger (overflow, dropdown).

**Droid does not wrap Material menu APIs at the current source pin.** Classic [`Menu`](../classic/menu.md) and popup menus exist separately — this page is about Material menu components only.

## Use it when…

Do not plan Material menu UI against Droid yet. Classic [`PopupMenu`](../classic/popup-menu.md) / [`ListPopupWindow`](../classic/list-popup-window.md) are the pin-backed popup surfaces today.

## How it behaves

There is no Material menu constructor, dropdown anchoring API, or Material menu listener bridge at this pin.

## Notes

This page will grow only after live Material menu wrappers land in a commit pin with real methods.
