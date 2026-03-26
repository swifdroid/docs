# Toolbar

The platform Android toolbar is currently exposed by Droid as `Toolbar1`.

The current wrapper is a substantial `ViewGroup`: it supports declarative child content, reactive title/subtitle text, toolbar icons and insets, menu/overflow operations, listener bridges, and lower-level layout/state helpers.

## Use it when…

Use `Toolbar1` when you intentionally want Android's platform `android.widget.Toolbar`.

Apps built around AppCompat or Material components will usually prefer their corresponding toolbar wrappers.

## Basic configuration

```swift
Toolbar1()
    .title("Inbox")
    .subtitle("3 unread")
    .popupTheme(1001)
    .contentInsetsRelative(start: 16, end: 16)
```

`title(_:)` and `subtitle(_:)` accept `String` and `State<String>`, so their queued values can keep following state changes after the Android instance is created.

## Initializers

```swift
Toolbar1()
Toolbar1(id: 1001)

Toolbar1 {
    TextView("Custom child")
}
```

The content initializer comes from the current `Toolbar1: ViewGroup` implementation.

## Queued toolbar configuration

The current wrapper queues these kinds of settings through Droid's view-property system:

- `popupTheme(_:)`
- `collapsible(_:)`
- `contentInsetStartWithNavigation(_:)`
- `contentInsetEndWithActions(_:)`
- `contentInsetsAbsolute(left:right:)`
- `contentInsetsRelative(start:end:)`
- `titleMargin(start:top:end:bottom:)` and individual title-margin setters
- drawable overloads of `collapseIcon(_:)`, `logo(_:)`, `navigationIcon(_:)`, and `overflowIcon(_:)`
- String overloads of collapse/logo/navigation content descriptions
- `titleTextColor(_:)` / `subtitleTextColor(_:)`
- `navigationOnClickListener(_:)`
- `onMenuItemClickListener(_:)`
- `titleTextAppearance(_:_:)` / `subtitleTextAppearance(_:_:)`

The listener and text-appearance methods accept JNI-compatible Android objects where required.

## Resource-ID overloads are live

Several similarly named `Int` overloads call the current Android instance immediately instead of queueing a property:

- `collapseIcon(_ iconResId: Int)`
- `collapseContentDescription(_ descriptionResId: Int)`
- `logo(_ logoResId: Int)`
- `logoDescription(_ descriptionResId: Int)`
- `navigationContentDescription(_ descriptionResId: Int)`
- `navigationIcon(_ iconResId: Int)`
- `subtitle(_ subtitleResId: Int)`
- `title(_ titleResId: Int)`

Use these resource-ID forms only when the toolbar already has a live Android instance.

## Menu and overflow

Live toolbar actions include:

- `inflateMenu(_:)`
- `showOverflowMenu()`
- `hideOverflowMenu()`
- `dismissPopupMenus()`
- `collapseActionView()`

Live state reads include:

- `menu()`
- `canShowOverflowMenu()`
- `isOverflowMenuShowing()`
- `isOverflowMenuShowPending()`
- `hasExpandedActionView()`
- `isTitleTruncated()`

`menuCallbacks(_:_:)` is also a live low-level bridge for Android menu presenter/builder callback objects.

## Reading toolbar state

The wrapper exposes live getters for:

- `title()` / `subtitle()`
- `logo()` / `logoDescription()`
- `navigationIcon()` / `navigationContentDescription()` / `navigationView()`
- `overflowIcon()`
- `popupTheme()`
- title margins
- configured and current content insets

## Lower-level helpers

`Toolbar1` also exposes layout-param generation/checking, measurement/layout callbacks, saved-state hooks, touch/attach/detach callbacks, and `wrapper()` for the internal Android wrapper object.

The public Swift type is currently named `Toolbar1`; use that exact name in source until the Droid wrapper itself is renamed.
