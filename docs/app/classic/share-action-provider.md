# ShareActionProvider

`ShareActionProvider` is the Android action provider that turns a menu item into the system share chooser.

Droid exposes the share-history file name and the share intent. Wrap the provider attached to a menu item, then set what should be shared. Visibility helpers come from [`ActionProvider`](action-provider.md).

## Use it when…

Use `ShareActionProvider` when a menu item should open Android’s share sheet for text, a link, or other payload the user is currently viewing.

Do not use it for custom in-app share buttons that skip the system chooser.

## How it behaves

The type wraps `android.widget.ShareActionProvider` and inherits [`ActionProvider`](action-provider.md) behavior such as `isVisible()` and `refreshVisibility()`.

`shareIntent(_:)` sets the payload Android offers to share targets. `shareHistoryFileName(_:)` names the file Android uses to remember target order; the default is `ShareActionProvider.defaultShareHistoryFileName`.

Configure these on a live provider after the menu item exists. The history file name can be set at any time, but only action views created afterward are backed by the new file.

## Examples

### Share text from a menu item

```swift
func attachShare(to item: MenuItem, text: String) {
    guard let base = item.actionProvider() else { return }
    let provider = ShareActionProvider(base.object)

    if let intent = Intent(.send) {
        intent.putExtra("android.intent.extra.TEXT", text)
        intent.type("text/plain")
        provider.shareIntent(intent)
    }
}
```

Android opens the share sheet when the user activates that menu item.

### Persist share-target order

```swift
provider.shareHistoryFileName("article_share_history.xml")
provider.shareIntent(intent)
```

Targets the user picked before move toward the top of the chooser. Use the default name when a screen has one share payload:

```swift
provider.shareHistoryFileName(ShareActionProvider.defaultShareHistoryFileName)
```

## Methods

### `shareIntent(_:)`

Sets the Android `Intent` that describes what to share.

```swift
provider.shareIntent(intent)
```

### `shareHistoryFileName(_:)`

Sets the file used to persist share-target history.

```swift
provider.shareHistoryFileName("share_history.xml")
```

### `defaultShareHistoryFileName`

Android’s default history file name (`share_history.xml`).

```swift
let name = ShareActionProvider.defaultShareHistoryFileName
```

## Notes

`setOnShareTargetSelectedListener` is not wrapped yet. Build the share `Intent` with the regular Intent helpers (`IntentActionType.send`, `putExtra(_:_)`, `type(_:)`).
