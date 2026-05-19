# Cards

Material cards group related content and actions on a surface with elevation and rounded corners.

**Droid does not wrap `MaterialCardView` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan card UI against Droid yet. Use a [`MaterialButton`](buttons/button.md) or a classic layout container when you need a grouped surface today.

## How it behaves

There is no `MaterialCardView()` constructor to place, no checked-card API, and no card listener bridge at this pin.

Android’s Material card API (stroke, checked state, shape appearance) must not be treated as available from Droid.

## Notes

This page will grow only after a live `MaterialCardView` wrapper lands in a commit pin with real methods.
