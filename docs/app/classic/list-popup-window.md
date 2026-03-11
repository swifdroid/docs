# ListPopupWindow

`ListPopupWindow` wraps Android's popup list around an existing JNI object. The current Droid wrapper exposes popup visibility, anchor/layout configuration, adapter and listener bridges, selection, keyboard handling, and access to the internal `ListView`.

## Use it when…

Use it when another Android API gives you a real `android.widget.ListPopupWindow` object and you need to configure or control that popup from Swift.

This wrapper is lower-level than normal Droid views: it does not construct a popup from an `ActivityContext` and it does not participate in the declarative view builder.

## How it behaves

Every setter and action operates immediately on the wrapped Android object.

There are no queued `ViewPropertyToApply` properties because `ListPopupWindow` is not a Droid `View` subclass.

## Example

```swift
func configurePopup(_ popup: ListPopupWindow, anchor: View) {
    popup
        .anchorView(anchor)
        .modal(true)
        .width(320)
        .horizontalOffset(8)

    popup.show()
}
```

The example assumes you already have a valid `ListPopupWindow` wrapping an Android object.

## Initializer

```swift
ListPopupWindow(androidObject)
```

The initializer accepts an existing `JObject`.

## Popup lifecycle

`show()`, `postShow()`, `dismiss()`, and `dismissImmediate()` control the popup.

`isShowing()` reports whether it is currently visible.

## Anchor and geometry

`anchorView(_:)` sets the anchor view. `anchorView()` reads it back.

The wrapper also exposes:

- `width(_:)` / `width()`
- `height(_:)` / `height()`
- `horizontalOffset(_:)` / `horizontalOffset()`
- `verticalOffset(_:)` / `verticalOffset()`
- `contentWidth(_:)`
- `dropDownGravity(_:)`
- `overlapAnchor(_:)`

## Adapter, selection, and internal list

`adapter(_:)` accepts a JNI-compatible Android `ListAdapter` object.

`selection(_:)`, `clearListSelection()`, `selectedItem()`, `selectedItemId()`, `selectedItemPosition()`, and `selectedView()` expose popup selection state.

`listView()` returns the live internal `ListView?` once Android has one available.

## Styling and input behavior

The wrapper exposes popup background, animation style, input-method mode, soft-input mode, prompt position/view, list selector, epicenter bounds, outside-touch behavior, and window layout type.

## Listener bridges

`onDismissListener(_:)`, `onItemClickListener(_:)`, and `onItemSelectedListener(_:)` accept JNI-compatible Android listener objects.

Droid exposes these bridges but does not construct the listener objects for you.

## Keyboard and drag-to-open helpers

`onKeyDown(_:_:)`, `onKeyUp(_:_:)`, and `onKeyPreIme(_:_:)` forward keyboard handling to the live popup.

`performItemClick(_:)` invokes a popup item, while `createDragToOpenListener(_:)` returns Android's drag-to-open touch-listener object when available.
