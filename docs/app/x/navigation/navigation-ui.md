# NavigationUI

`NavigationUI` keeps an AppCompat action bar aligned with a `NavController` and handles Up navigation and menu destination selection. Use the same [AppBarConfiguration](app-bar-configuration.md) for action-bar setup and Up handling so both follow your top-level destination choices.

The helpers are static: you do not need to create a `NavigationUI` instance. Call them from the main actor.

## Use it when…

Use it when an AppCompat activity's action-bar title and navigation button should follow the current destination, or when a menu selection should navigate using the item's ID.

## How it behaves

Action-bar setup updates the title when the destination has a valid label. The configuration controls the navigation button, and `FloatingWindow` destinations are ignored. Setup does not handle the Up action for you; delegate that action separately through `navigateUp`.

Menu selection requires an item ID that matches a valid action or destination ID. By default, selection pops the back stack to the graph's start destination. Items with `android:menuCategory="secondary"` do not pop the back stack.

## Examples

These functions accept the activity, controller, and configuration already used by your screen. Call the setup function when connecting the action bar, and the Up function when handling the Up action.

```swift
@MainActor
func configureActionBar(
    activity: AppCompatActivity,
    navController: NavController,
    configuration: AppBarConfiguration
) {
    NavigationUI.setupActionBarWithNavController(
        activity,
        navController,
        configuration
    )
}

@MainActor
func handleUp(
    navController: NavController,
    configuration: AppBarConfiguration
) -> Bool {
    return NavigationUI.navigateUp(navController, configuration)
}
```

For a menu-selection handler, pass the selected item and the controller that owns its destination:

```swift
@MainActor
func handleDestinationSelection(
    item: MenuItem,
    navController: NavController
) -> Bool {
    return NavigationUI.onNavDestinationSelected(item, navController)
}
```

## Initializers

`NavigationUI` is an open, main-actor-isolated `JObjectable` class with an `object: JObject` property. If you need an instance, `NavigationUI(existingObject)` accepts an existing `JObject`, while `NavigationUI(context)` accepts a `Contextable` and can return `nil` if Java class loading or object creation fails. Neither initializer is required for the static calls above.

## Methods

### Connect the action bar

`setupActionBarWithNavController` takes the hosting `AppCompatActivity`, its navigation controller, and the configuration that controls the navigation button:

```swift
NavigationUI.setupActionBarWithNavController(activity, navController, configuration)
```

This call returns no result. It returns without setting up the action bar if the activity context or Java class is unavailable.

### Handle Up

`navigateUp` uses the supplied configuration when delegating Up handling to the controller:

```swift
let handled = NavigationUI.navigateUp(navController, configuration)
```

The Boolean reports whether Up was handled. A `false` result also occurs if Java class loading fails or the Java call produces no Boolean result.

### Navigate from a menu item

`onNavDestinationSelected` attempts navigation for the selected item's ID and returns whether the navigation attempt succeeded:

```swift
let handled = NavigationUI.onNavDestinationSelected(item, navController)
let handledWithState = NavigationUI.onNavDestinationSelected(item, navController, true)
```

The optional third argument is `saveState: Bool?`, defaulting to `nil`. With `nil`, the Java call omits that argument; a supplied `true` or `false` is passed through. A `false` result can also indicate failed Java class loading or a missing Boolean result.

## Notes

The Swift `AppBarConfiguration.Builder` does not expose fallback-listener configuration.
