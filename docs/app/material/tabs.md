# Tabs

Material tabs switch between related sections on one screen (genres, days, account views).

**Droid does not wrap `TabLayout` or `TabItem` at the current source pin.** The JNI names exist in the package namespace, but the Swift wrapper types are not live.

## Use it when…

Do not plan tab UI against Droid yet. Use a horizontal row of [`MaterialButton`](buttons/button.md) controls or navigation destinations you drive yourself.

## How it behaves

There is no `TabLayout()` constructor to place, no tab selection API, and no `TabLayout` listener bridge at this pin.

Android’s Material tab API (tabs, badges on tabs, `ViewPager` / `ViewPager2` linkage) must not be treated as available from Droid.

## Notes

This page will grow only after live `TabLayout` / `TabItem` wrappers land in a commit pin with real methods.
