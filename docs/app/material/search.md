# Search

Material search is the search-bar / search-view pattern used in Material app bars.

**Droid does not wrap Material search APIs at the current source pin.** Classic [`SearchView`](../classic/search-view.md) is a placeable control, but its query APIs are not wrapped there either.

## Use it when…

Do not plan Material search chrome against Droid yet. Drive a simple text field and your own results list.

## How it behaves

There is no Material search-bar constructor, expand/collapse API, or suggestion bridge at this pin.

## Notes

This page will grow only after a live Material search wrapper lands in a commit pin with real methods.
