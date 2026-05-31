# ComponentActivity

`ComponentActivity` provides lower-level building blocks for composing higher-level activity components. It keeps that foundation small so components can work together without a deep activity hierarchy or strong coupling.

Its exposed helpers let you add an existing view, initialize view-tree owners, refresh a menu after a state change, and enable edge-to-edge display.

## Use it when…

Use `ComponentActivity` when you are composing activity components and need these content, menu, or display helpers.

## How it behaves

`ComponentActivity` is an open, main-actor-isolated subclass of `Activity`. The inherited `Activity.onCreate` calls `body()` and then `buildUI()` when it builds the content.

All four helpers log and return when the activity context is missing. `addContentView` also returns immediately if the supplied view has no underlying instance. `enableEdgeToEdge` returns with a debug log if the AndroidX edge-to-edge class cannot be loaded.

## Examples

### Display a ready message

```swift
import Droid

final class ReadyActivity: ComponentActivity {
    override var body: BodyBuilder.Result {
        TextView("Ready")
    }
}
```

### Install existing content

This function accepts a view and layout params that already exist. Initialize the view-tree owners before adding the content so inflation and attach listeners see the owners already present.

```swift
@MainActor
func installContent(
    in activity: ComponentActivity,
    view: View,
    layoutParams: LayoutParams
) {
    activity.initializeViewTreeOwners()
    activity.addContentView(view, layoutParams)
}
```

## Methods

### `addContentView`

Adds an existing view with the supplied layout params through the underlying activity call. The view must have an underlying instance; otherwise the call returns without adding it.

```swift
@MainActor
func addContent(to activity: ComponentActivity, view: View, layoutParams: LayoutParams) {
    activity.addContentView(view, layoutParams)
}
```

### `initializeViewTreeOwners()`

Sets view-tree owners before content is set. Call it before installing content, as in the example above.

```swift
@MainActor
func prepareContent(in activity: ComponentActivity) {
    activity.initializeViewTreeOwners()
}
```

### `invalidateMenu()`

Invalidates the menu so its displayed state matches its current internal state. Call it after a menu state change, such as removing or disabling an item in response to a user event.

```swift
@MainActor
func menuStateDidChange(in activity: ComponentActivity) {
    activity.invalidateMenu()
}
```

### `enableEdgeToEdge()`

Enables edge-to-edge display for the activity. If `androidx/activity/EdgeToEdge` cannot be loaded, the call logs a debug message and returns.

```swift
@MainActor
func enableEdgeToEdgeDisplay(in activity: ComponentActivity) {
    activity.enableEdgeToEdge()
}
```
