# ActionMenuView

`ActionMenuView` is Droid's wrapper for Android's toolbar-style action menu container. It inherits `LinearLayout`, supports declarative child content, and exposes live menu, overflow, popup-theme, presenter, callback, and layout-parameter operations.

## Use it when…

Use `ActionMenuView` when you specifically need Android's action-menu/overflow behavior.

For an ordinary horizontal row of controls, `LinearLayout` is simpler.

## How it behaves

The current action-menu-specific setters call the live Android instance immediately.

They return `Self` where convenient, but they are not queued `ViewPropertyToApply` properties.

The declarative content initializer itself is available because `ActionMenuView` explicitly overrides it.

## Example

### Declare child content

```swift
ActionMenuView {
    Button("Edit")
    Button("Share")
}
```

### Control a live overflow menu

```swift
let actions = ActionMenuView()

actions.showOverflowMenu()
actions.hideOverflowMenu()
```

Overflow operations require a live Android instance.

## Initializers

```swift
ActionMenuView()
ActionMenuView(id: 1001)

ActionMenuView {
    Button("Action")
}
```

## Menu access

`menu()` returns the live `Menu?` exposed by Android.

`peekMenu()` returns the underlying implementation object without requiring the higher-level `Menu` wrapper.

`invokeItem(_:)` forwards a menu-item implementation object to Android.

## Overflow menu

The wrapper exposes:

- `showOverflowMenu()`
- `hideOverflowMenu()`
- `dismissPopupMenus()`
- `isOverflowMenuShowing()`
- `isOverflowMenuShowPending()`
- `isOverflowReserved()`

`overflowIcon()` reads the current icon, while `overflowIcon(_:)` sets it on the live instance.

## Theme, presenter, and callbacks

`popupTheme(_:)`, `overflowReserved(_:)`, `presenter(_:)`, `expandedActionViewsExclusive(_:)`, and `menuCallbacks(_:_:)` are live setters.

`onMenuItemClickListener(_:)` accepts a JNI-compatible Android `ActionMenuView.OnMenuItemClickListener` object.

Droid exposes the listener bridge but does not construct the listener object for you.

## Layout helpers

The wrapper exposes Android's action-menu layout-param generation/checking helpers plus measurement, layout, accessibility, and configuration-change hooks for lower-level integrations.