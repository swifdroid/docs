# Toolbar (AppCompat)

`Toolbar` is an app bar for use within application content. Its direct methods let you refresh an existing menu, inspect overflow and expanded action views, and forward events or direction-change callbacks.

Keep a reference to the toolbar when application state changes need a menu refresh or when you need to inspect its current menu state.

## Use it when…

Use it when your screen has an AppCompat toolbar and application events change which menu items should be displayed or enabled.

Use its state queries when you need to inspect overflow visibility, an expanded action view, or whether the toolbar will attempt back-callback registration in supported configurations. Event-forwarding methods are useful when you already have the corresponding event or direction callback to pass along.

## How it behaves

Run these calls on the main actor. Each Boolean method forwards the Java result when an instance exists, or returns `false` when it does not. A pre-instantiation `false` therefore does not establish the toolbar's Android state. The two methods without a result, `invalidateMenu()` and `onRtlPropertiesChanged(_:)`, skip forwarding when no instance exists.

Menu invalidation refreshes items supplied by menu providers after menu state changes. Manually inflated items remain manually managed. Overflow visibility is a current-state query and may not reflect a pending show operation.

## Examples

### Refresh after application menu state changes

After your application changes existing menu state, such as disabling an item in response to a user action, request a refresh on its instantiated toolbar. This call returns no value.

```swift
@MainActor
func refreshMenu(afterAppMenuStateChange toolbar: Toolbar) {
    toolbar.invalidateMenu()
}
```

### Request that overflow items be hidden

When your application needs to dismiss overflow items on an instantiated toolbar, pass it to this helper. The helper returns the Boolean from `hideOverflowMenu()` as-is; the Swift method does not define it as success or as a change in visibility.

```swift
@MainActor
func hideOverflowItems(on toolbar: Toolbar) -> Bool {
    return toolbar.hideOverflowMenu()
}
```

## Methods

### Menu refresh and overflow

`invalidateMenu()` requests that the displayed menu reflect its current state. Call it after an application menu-state change. Only items supplied by menu providers should be removed and repopulated; manually inflated items remain under your application's management.

```swift
@MainActor
func refreshChangedMenu(on toolbar: Toolbar) {
    toolbar.invalidateMenu()
}
```

`isOverflowMenuShowing()` queries whether the overflow menu is currently showing. A pending show operation may not yet appear in this result.

```swift
@MainActor
func inspectOverflow(on toolbar: Toolbar) -> Bool {
    let showing = toolbar.isOverflowMenuShowing()
    return showing
}
```

`hideOverflowMenu()` requests hiding the associated menu's overflow items. Its Boolean is forwarded without a success or state-change interpretation defined by this Swift method.

```swift
@MainActor
func requestOverflowHide(on toolbar: Toolbar) -> Bool {
    let hideResult = toolbar.hideOverflowMenu()
    return hideResult
}
```

### Expanded action views and back invocation

`hasExpandedActionView()` checks whether this toolbar currently hosts an expanded action view.

```swift
@MainActor
func inspectExpandedActionView(on toolbar: Toolbar) -> Bool {
    let expanded = toolbar.hasExpandedActionView()
    return expanded
}
```

`isBackInvokedCallbackEnabled()` reports whether the toolbar will attempt to register its own back-invocation callback in supported configurations to collapse expanded action items. The result does not establish support in every configuration.

```swift
@MainActor
func inspectBackCallback(on toolbar: Toolbar) -> Bool {
    let callbackEnabled = toolbar.isBackInvokedCallbackEnabled()
    return callbackEnabled
}
```

### Event and direction callbacks

`onHoverEvent(_:)` forwards an existing `MotionEvent` to the toolbar's Java hover-event method, identifying the argument as a `MotionEvent`. Return the forwarded Boolean to the caller without assigning additional event-consumption meaning to it.

```swift
@MainActor
func forwardHover(to toolbar: Toolbar, event: MotionEvent) -> Bool {
    let hoverResult = toolbar.onHoverEvent(event)
    return hoverResult
}
```

`onTouchEvent(_:)` forwards an existing `MotionEvent` to the corresponding Java touch-event method with the same argument type. This helper returns its Boolean result.

```swift
@MainActor
func forwardTouch(to toolbar: Toolbar, event: MotionEvent) -> Bool {
    let touchResult = toolbar.onTouchEvent(event)
    return touchResult
}
```

`onRtlPropertiesChanged(_:)` forwards a direction-change callback using the supplied `LayoutDirection` value's raw value. It returns no result. Use it to forward an existing callback; it is not a layout-direction setter and does not itself promise a visible layout change.

```swift
@MainActor
func forwardDirectionChange(to toolbar: Toolbar, direction: LayoutDirection) {
    toolbar.onRtlPropertiesChanged(direction)
}
```
