# Dialogs

Material dialogs interrupt for decisions, confirmations, and short input.

**Droid does not wrap Material dialog APIs at the current source pin.** There is no live Material dialog builder or dialog Swift type to show.

## Use it when…

Do not plan Material dialog UI against Droid yet. Drive choices with on-screen controls ([`MaterialButton`](buttons/button.md), [`RadioGroup`](../classic/radio-group.md)) until a dialog wrapper lands.

## How it behaves

There is no dialog constructor, title/body/actions API, or dialog listener bridge at this pin.

Android’s Material dialog API (alert dialogs, simple list dialogs, theming) must not be treated as available from Droid.

## Notes

This page will grow only after a live dialog wrapper lands in a commit pin with real methods.
