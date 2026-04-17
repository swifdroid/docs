# ListPopupWindow

`ListPopupWindow` is a dropdown list anchored to a view. Android uses it for spinner-style choices, autocomplete panels, and any compact list that should appear next to the control that triggered it.

In Droid, `ListPopupWindow` is a live object wrapper. It is not a view you place in a layout builder. You receive an existing `android.widget.ListPopupWindow` from Android, then configure its anchor, size, adapter, and selection from Swift.

Every setter and action runs immediately on the wrapped Android object. There is no queued layout-property pipeline because this type is not a Droid `View` subclass.

## Use it when…

Use `ListPopupWindow` when another Android API hands you a real popup list and you need to show, dismiss, or reconfigure that dropdown from Swift.

Do not use it as a general list screen. For large or persistent collections, use a list destination or a recycled list component.

## How it behaves

The popup attaches to an anchor view. Width, height, and pixel offsets control how the dropdown fits under that anchor. Gravity and overlap settings decide alignment when space is tight.

Items come from an Android `ListAdapter`. Selection APIs read or move the highlighted row, and `listView()` returns the internal list once Android has created one.

Show and dismiss are live operations. `show()` opens immediately; `postShow()` posts the show work to the Android UI thread. `dismiss()` uses the usual dismiss path; `dismissImmediate()` closes the popup immediately without that usual path. `isShowing()` reports current visibility.

Item-click, item-selected, and dismiss listeners accept Android/JNI-compatible listener objects. Droid does not wrap those events in Swift closures.

## Examples

### Open a dropdown under a text field

```swift
func showChoices(_ popup: ListPopupWindow, anchor: View) {
    popup
        .anchorView(anchor)
        .modal(true)
        .width(320)
        .horizontalOffset(8)

    popup.show()
}
```

The list appears under the field and closes when the user taps outside.

### Move selection from application state

```swift
func selectSecondRow(_ popup: ListPopupWindow) {
    popup.selection(1)
}

func readSelection(_ popup: ListPopupWindow) -> Int {
    popup.selectedItemPosition()
}
```

`selection(_:)` highlights a row. `selectedItemPosition()` reports the current index.

### Close after a commit

```swift
func onRowChosen(_ popup: ListPopupWindow) {
    popup.dismiss()
}
```

Dismiss the dropdown as soon as the user commits so the anchor control is usable again.

## Initializers

```swift
ListPopupWindow(androidObject)
```

The initializer accepts an existing JNI object for `android.widget.ListPopupWindow`. Droid does not construct this popup from an activity context or a view-builder block.

## Methods

### Popup lifecycle

`show()` opens the dropdown. `postShow()` schedules showing on the Android message queue. `dismiss()` closes it. `dismissImmediate()` closes without the usual animation path. `isShowing()` reports visibility.

```swift
popup.show()
popup.postShow()
popup.dismiss()
popup.dismissImmediate()
```

### Anchor and geometry

`anchorView()` reads the anchor. `anchorView(_:)` sets it. Width, height, and offsets are available as `width`, `height`, `horizontalOffset`, and `verticalOffset` getters/setters. `contentWidth(_:)` sets content width. `dropDownGravity(_:)` and `overlapAnchor(_:)` tune alignment.

```swift
popup
    .anchorView(button)
    .width(320)
    .height(240)
    .horizontalOffset(8)
    .verticalOffset(0)
```

### Adapter, selection, and internal list

`adapter(_:)` accepts a JNI-compatible Android `ListAdapter`. `selection(_:)` selects a row. `clearListSelection()` clears the highlight. `selectedItem()`, `selectedItemId()`, `selectedItemPosition()`, and `selectedView()` read selection state. `listView()` returns the live internal `View?` wrapping Android’s list when available.

```swift
popup.adapter(listAdapter)
popup.selection(0)

let index = popup.selectedItemPosition()
let list = popup.listView()
```

### Styling and input behavior

`backgroundDrawable(_:)` sets the popup background. `animationStyle(_:)` sets the enter/exit animation. `inputMethodMode(_:)` and `softInputMode(_:)` control IME behavior. `listSelector(_:)` sets the row selector drawable. `promptPosition(_:)` / `promptView(_:)` configure the optional prompt. `epicenterBounds(_:)` and `windowLayoutType(_:)` tune window placement. `modal(_:)`, `dropDownAlwaysVisible(_:)`, and `forceIgnoreOutsideTouch(_:)` adjust outside-touch behavior.

```swift
popup
    .modal(true)
    .animationStyle(style)
    .listSelector(selector)
```

### Listeners

`onDismissListener(_:)`, `onItemClickListener(_:)`, and `onItemSelectedListener(_:)` attach Android-compatible listener objects.

```swift
popup.onDismissListener(dismissListener)
popup.onItemClickListener(clickListener)
popup.onItemSelectedListener(selectedListener)
```

### Keyboard and drag-to-open helpers

`onKeyDown(_:_:)`, `onKeyUp(_:_:)`, and `onKeyPreIme(_:_:)` forward key events to the live popup. `performItemClick(_:)` invokes a popup item by position. `createDragToOpenListener(_:)` returns Android’s drag-to-open touch-listener object when available.

```swift
let handled = popup.performItemClick(0)
let dragListener = popup.createDragToOpenListener(anchor)
```

## Notes

Configure anchor and adapter before the first `show()`. Use `isShowing()` before relying on `listView()` or selection reads so Android has created the internal list.
