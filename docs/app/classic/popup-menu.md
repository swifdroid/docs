# PopupMenu

`PopupMenu` shows a small anchored list of menu items above or below a view. Android uses this pattern for overflow actions, row menus, and compact “more” controls that should not take over the whole screen.

In Droid, `PopupMenu` is a live object wrapper. It is not a layout node. You receive an existing `android.widget.PopupMenu` from Android, configure gravity, inflate or build items, then `show()` and `dismiss()` that popup from Swift.

Keep it for short action lists tied to a specific anchor. For full-screen navigation or long scrolling choices, use a dialog, sheet, or list screen instead.

## Use it when…

Use `PopupMenu` when a button or row needs a compact list of actions that appear next to the control the user just tapped.

Do not use it as a general menu system. If every screen needs the same top-level destinations, prefer an app bar menu or navigation drawer.

## How it behaves

The popup anchors to the view Android associates with it. `gravity(_:)` controls how the list aligns relative to that anchor. `show()` displays the popup; `dismiss()` closes it.

Items come from a `Menu?`. You can inflate a menu resource with `inflate(_:)`, or obtain `menu()` and work with the live menu object when it exists. `forceShowIcon` keeps item icons visible even when Android would normally hide them.

`menuListView()` returns a `View?` wrapping the internal list once Android has created it. `dragToOpenListener()` returns Android’s drag-to-open touch listener so a long drag on the anchor can open the same popup.

Dismiss and item-click listeners accept Android/JNI-compatible listener objects. Droid does not wrap those events in Swift closures.

## Examples

### Show actions from a row button

```swift
func showRowActions(_ popup: PopupMenu) {
    popup.show()
}
```

Tapping the row’s “more” control displays the menu anchored to that button.

### Align and force icons

```swift
func configureOverflow(_ popup: PopupMenu) {
    popup
        .gravity(0)
        .forceShowIcon(true)

    popup.show()
}
```

The list stays icon-visible for scanable action rows.

### Dismiss after a command

```swift
func onCommandChosen(_ popup: PopupMenu) {
    popup.dismiss()
}
```

Close the popup as soon as the user commits to an action so the anchor control does not stay covered.

## Initializers

```swift
PopupMenu(androidObject)
```

The initializer accepts an existing JNI object for `android.widget.PopupMenu`. Droid does not build a popup menu from a view-builder block.

## Methods

### Show and dismiss

`show()` opens the menu. `dismiss()` closes it.

```swift
popup.show()
popup.dismiss()
```

### Gravity

`gravity()` reads the current alignment. `gravity(_:)` sets alignment relative to the anchor and returns the same popup for chaining.

```swift
popup.gravity(0)
```

### Menu content

`menu()` returns the live `Menu?`. `inflate(_:)` fills the popup from a menu resource id. `forceShowIcon(_:)` keeps icons visible. `menuInflater()` returns Android’s inflater for this popup.

```swift
popup.inflate(menuResId)
popup.forceShowIcon(true)

if let menu = popup.menu() {
    menu.add("Rename")
}
```

### Internal list and drag-to-open

`menuListView()` returns the popup’s `ListView` when available. `dragToOpenListener()` returns the touch listener that opens the popup on drag.

```swift
let list = popup.menuListView()
let dragListener = popup.dragToOpenListener()
```

### Listeners

`onDismissListener(_:)` attaches a JNI-compatible dismiss listener. `onMenuItemClickListener(_:)` attaches a JNI-compatible item-click listener.

```swift
popup.onDismissListener(dismissListener)
popup.onMenuItemClickListener(clickListener)
```

Both methods accept Android-compatible listener objects, not Swift closures.

## Notes

Icons stay optional unless you call `forceShowIcon(true)`. Close the popup after a committed action so the user can continue interacting with the underlying screen.
