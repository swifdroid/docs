# Menu

`Menu` contains the actions shown by an Activity options menu, including toolbar actions and overflow items.

Build the menu from the Activity lifecycle, update state-dependent items before display, and handle selections separately.

## Use it when…

Use a menu for a small set of screen-level actions such as Refresh, Search, Share, or Settings.

Keep primary workflow actions in the screen itself when hiding them in overflow would make the interface harder to discover.

## How it behaves

`onCreateOptionsMenu(menu:)` is the normal place to create items. Android can call `onPrepareOptionsMenu(menu:)` again before display, which makes it suitable for state-dependent changes.

Selections arrive through `onOptionsItemSelected(item:)`.

## Examples

### Add toolbar and overflow actions

```swift
override func onCreateOptionsMenu(menu: Menu) -> Bool {
    menu.add("Search").showAsAction(.always)
    menu.add("Settings").showAsAction(.never)
    return true
}
```

### Rebuild state-dependent items

```swift
override func onPrepareOptionsMenu(menu: Menu) -> Bool {
    menu.clear()
    menu.add(isConnected ? "Disconnect" : "Connect")
    return true
}
```

### Add a submenu

```swift
override func onCreateOptionsMenu(menu: Menu) -> Bool {
    let more = menu.addSubMenu("More")
    more?.add("Help")
    more?.add("About")
    return true
}
```

## Methods

### Add an item

Use `add(_:)` for a title-only item, or the identified form when later code needs a stable id.

```swift
menu.add("Refresh")
menu.add(groupId: 0, itemId: 100, order: 0, title: "Settings")
```

### Add a submenu

```swift
let more = menu.addSubMenu("More")
more?.add("Help")
```

### Clear or find items

```swift
menu.clear()

let settings = menu.findItem(100)
```
