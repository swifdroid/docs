# ActionProvider

`ActionProvider` wraps an Android action provider associated with a `MenuItem`.

Droid lets you inspect provider visibility and submenu state, refresh item visibility, and use the supported configuration on `ShareActionProvider` and `MediaRouteActionProvider`.

## Use it when…

Use `ActionProvider` when a menu item already has an Android action provider and your Swift code needs to inspect or configure that provider.

## Inspect a menu item's provider

`MenuItem.actionProvider()` returns the provider currently attached to the item:

```swift
if let provider = item.actionProvider() {
    print(provider.hasSubMenu())
    print(provider.isVisible())

    if provider.overridesItemVisibility() {
        provider.refreshVisibility()
    }
}
```

`MenuItem.actionProvider(_:)` can also attach an `ActionProvider` instance to a menu item.

## Configure sharing

When you already have a `ShareActionProvider`, configure its history file and share intent directly:

```swift
func configureShare(
    _ provider: ShareActionProvider,
    intent: Intent
) {
    provider.shareHistoryFileName("share_history.xml")
    provider.shareIntent(intent)
}
```

`defaultShareHistoryFileName` exposes Android's default share-history filename.

`MediaRouteActionProvider.routeTypes(_:)` sets the route type mask used by the Android media-route chooser.

## Current limitations

`onPrepareSubMenu(_:)`, `MediaRouteActionProvider.onCreateActionView(_:)`, and `MediaRouteActionProvider.onPerformDefaultAction()` currently do not perform Android work. Do not rely on those hooks.
