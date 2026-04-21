# OverScroller

`OverScroller` is Android’s fling and scroll-physics helper. It is not a widget you place on screen; it computes animated coordinates that custom scrolling code then applies.

Droid does not expose an `OverScroller` wrapper in the current source pin. The type and its methods are not part of the public Swift surface yet.

## Use it when…

Do not plan screen work against Droid `OverScroller` until a wrapper lands in a source release and this page documents a verified method set.

For ordinary scrolling containers, use [`ScrollView`](scroll-view.md) or a layout that already scrolls.

## How it behaves

There is no Droid `OverScroller` type to construct, no `startScroll` / `fling` / `computeScrollOffset` API to call, and no existing-object initializer for this helper at the current pin.

Android upstream documents a rich scroller API. That upstream surface must not be treated as available from Droid.

## Notes

Earlier drafts of this page described wrapper methods that were never part of the pinned Droid source. Those claims were removed on purpose. This entry will grow only after `OverScroller` appears in a commit pin with real methods.
