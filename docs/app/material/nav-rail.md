# Navigation rail

Material navigation rail is the side navigation strip for large-screen layouts (3–7 destinations).

**Droid does not wrap `NavigationRailView` at the current source pin.** The JNI name exists in the package namespace, but the Swift wrapper type is not live.

## Use it when…

Do not plan navigation-rail UI against Droid yet. [`BottomNavigationView`](nav-bar.md) is the pin-backed Material destination bar available today.

## How it behaves

There is no `NavigationRailView()` constructor to place, no menu/badge API, and no rail listener bridge at this pin.

Android’s Material navigation-rail API (header, item badges, label visibility) must not be treated as available from Droid.

## Notes

This page will grow only after a live `NavigationRailView` wrapper lands in a commit pin with real methods.
