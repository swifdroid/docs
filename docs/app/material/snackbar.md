# Snackbar

A `Snackbar` is a short message that appears at the bottom of the screen, optionally with one action (“Undo”, “Retry”).
It’s great for confirming an operation without interrupting the user.

In Droid, you create one with `Snackbar.make(...)`, optionally add an action, then call `show()`.

## Use it when…

Use it when the user should notice something, but you don’t want a modal dialog: “Saved”, “Moved to trash”, “Offline”.

Use it when there’s a quick recovery action: Undo delete, Retry network request, Reconnect.

## How it behaves

Snackbars sit above your content and dismiss themselves after a duration unless you choose `.indefinite`.
Only one snackbar is shown at a time; a new one replaces the old.

If your layout uses `CoordinatorLayout`, Android can automatically move other UI (like a FAB) so the snackbar doesn’t cover it.

## Examples

### Undo after delete

```swift
Snackbar.make(contentView!, "Item removed", .long)?
    .action("Undo") { restoreLastItem() }
    .show()
```

### Retry after a failed upload

```swift
Snackbar.make(contentView!, "Upload failed", .long)?
    .action("Retry") { uploadAgain() }
    .show()
```

### Quiet confirmation

```swift
Snackbar.make(contentView!, "Copied", .short)?.show()
```

## Methods

### Create

```swift
let api = Snackbar.make(contentView!, "Saved", .short)
```

### Action

```swift
Snackbar.make(contentView!, "Draft deleted", .long)?
    .action("Undo") { restoreDraft() }
    .actionTextColor(.white)
    .show()
```

### Styling

```swift
Snackbar.make(contentView!, "Offline", .long)?
    .backgroundTint(.black)
    .textColor(.white)
    .textMaxLines(2)
    .animationMode(.fade)
    .show()
```

## Notes

`contentView` is the easiest anchor view in an Activity. If it’s nil, pass any view that’s already in your hierarchy.

`text(_:)`, `text(resId:)`, and `maxInlineActionWidth(_:)` exist on the type but currently call the wrong Android method (`setBackgroundTint`) at this pin. Prefer setting message text through `Snackbar.make(...)` and avoid those three until a later pin fixes them.

`duration()` and `isShown()` read live state. `dismiss()` hides a visible snackbar.
