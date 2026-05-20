# Text fields

Material text fields wrap input with floating labels, helper text, error text, and outlined/filled styles.

**Droid does not wrap `TextInputLayout` or `TextInputEditText` at the current source pin.** The JNI names exist in the package namespace, but the Swift wrapper types are not live.

## Use it when…

Do not plan Material text-field chrome against Droid yet. Use classic [`EditText`](../classic/edit-text.md) for typing, with `hint(_:)` and shared text/view configuration.

## How it behaves

There is no `TextInputLayout()` / `TextInputEditText()` constructor to place, no floating-label API, and no counter/error helpers at this pin.

Android’s Material text-field API (start/end icons, password toggle, prefix/suffix) must not be treated as available from Droid.

## Notes

This page will grow only after live `TextInputLayout` / `TextInputEditText` wrappers land in a commit pin with real methods.
