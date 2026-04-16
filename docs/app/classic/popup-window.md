# PopupWindow

`PopupWindow` is a floating panel that sits above the current window. Android uses it for tooltips, custom dropdowns, lightweight editors, and any UI that must appear near a control without replacing the screen.

In Droid, `PopupWindow` is a live object wrapper. It is not a layout node. You receive an existing `android.widget.PopupWindow` from Android, set its content and window flags, then show it at a location or drop it under an anchor.

Prefer this when the panel is temporary and tightly coupled to a control on screen. For full workflows, use a dialog fragment or a separate destination.

## Use it when…

Use `PopupWindow` when a custom floating panel must appear over the current UI: anchored dropdowns, suggestion cards, or compact editors.

Do not use it when the user needs a full form or long scrolling content. Those belong in dialogs or dedicated screens.

## How it behaves

The popup hosts one content view. `contentView(_:)` assigns that view; `width` and `height` size the window. `showAsDropDown` places it under an anchor, while `showAtLocation` places it at a gravity-relative point.

Window flags decide focus and touch behavior. A focusable popup can take keyboard input; `outsideTouchable` and `touchModal` control whether touches outside dismiss or pass through. `clippingEnabled` keeps the popup on screen.

`isShowing()` reports visibility. `dismiss()` closes the popup. `onDismissListener` and `touchInterceptor` accept Android/JNI-compatible listener objects rather than Swift closures.

## Examples

### Drop a panel under a button

```swift
func showTooltip(_ popup: PopupWindow, _ anchor: View) {
    popup
        .contentView(tooltipView)
        .width(280)
        .height(120)
        .focusable(true)
        .outsideTouchable(true)

    popup.showAsDropDown(anchor)
}
```

The panel appears under the button and closes when the user taps outside.

### Place a popup at a parent origin

```swift
func showNearParentOrigin(_ popup: PopupWindow, _ parent: View) {
    popup
        .contentView(card)
        .width(320)
        .height(200)

    popup.showAtLocation(parent, 0, 0, 0)
}
```

Android positions the popup relative to `parent` using the supplied gravity and offsets.

### Dismiss after a choice

```swift
func onOptionPicked(_ popup: PopupWindow) {
    popup.dismiss()
}
```

Close the popup once the user commits so the underlying screen stays interactive.

## Initializers

```swift
PopupWindow(androidObject)
```

The initializer accepts an existing JNI object for `android.widget.PopupWindow`. Droid does not build a popup window from a view-builder block.

## Methods

### Show and dismiss

`showAsDropDown(_:)` anchors the popup under a view. `showAsDropDown(_:_:_:)` adds pixel offsets. `showAsDropDown(_:_:_:_:)` also applies gravity. `showAtLocation(_:_:_:_:)` places the popup at a gravity-relative point. `dismiss()` closes it.

```swift
popup.showAsDropDown(anchor)
popup.showAsDropDown(anchor, 0, 8)
popup.dismiss()
```

`isShowing()` reports whether the popup is currently visible.

### Content and size

`contentView()` reads the hosted view. `contentView(_:)` assigns it. `width()` / `width(_:)` and `height()` / `height(_:)` get or set window size. `windowLayoutMode(widthSpec:heightSpec:)` applies Android measure specs. `background()` reads the background drawable; `backgroundDrawable(_:)` sets it. `elevation()` / `elevation(_:)` get or set shadow elevation.

```swift
popup
    .contentView(panel)
    .width(300)
    .height(200)
    .elevation(8)

popup.backgroundDrawable(bg)
popup.windowLayoutMode(widthSpec: widthSpec, heightSpec: heightSpec)
let currentBg = popup.background()
```

### Focus, touch, and screen flags

`focusable(_:)` decides whether the popup takes keyboard focus. `touchable`, `touchModal`, and `outsideTouchable` shape hit testing. `clippingEnabled`, `clipToScreenEnabled`, `isClippedToScreen`, `layoutInScreenEnabled`, `layoutInsetDecor`, `attachedInDecor`, `splitTouchEnabled`, `overlapAnchor`, and `dropDown` tune window embedding.

```swift
popup
    .focusable(true)
    .outsideTouchable(true)
    .touchModal(false)

popup.clippingEnabled(true)
popup.overlapAnchor(true)
popup.dropDown(true)
```

Matching `is*` getters report the current flag values (`isFocusable`, `isOutsideTouchable`, `isShowing`, and the other flag reads exposed on this surface). `isClippedToScreen` is also a setter on this surface.

### Input and window type

`inputMethodMode` / `softInputMode` control IME behavior. `windowLayoutType` selects the Android window type used when the popup attaches.

```swift
popup
    .inputMethodMode(mode)
    .softInputMode(softMode)
    .windowLayoutType(layoutType)
```

### Geometry helpers

`maxAvailableHeight(anchor:)` and its offset variants report how much space remains below an anchor. `updateAboveAnchor(_:)` updates above-anchor state. `createPopupLayoutParams` and `findDropDownPosition` expose Android’s placement parameter helpers for advanced cases.

```swift
let available = popup.maxAvailableHeight(anchor: button, yOffset: 8)
popup.updateAboveAnchor(true)

let params = popup.createPopupLayoutParams(token)
```

### Transitions and listeners

`enterTransition` / `exitTransition` get or set Android transition objects. `epicenterBounds` / `transitionEpicenter` tune transition origin. `onDismissListener(_:)` attaches a JNI-compatible dismiss listener. `touchInterceptor(_:)` attaches a JNI-compatible touch interceptor.

```swift
popup.enterTransition(enter)
popup.exitTransition(exit)
popup.epicenterBounds(bounds)

popup.onDismissListener(dismissListener)
popup.touchInterceptor(interceptor)
```

Listeners are Android-compatible objects, not Swift closures.

## Notes

Set `contentView` before the first show. A focusable popup should also consider `outsideTouchable` so the user can leave the panel without a dedicated close control.
