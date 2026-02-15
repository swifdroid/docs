# NavigationUI

`NavigationUI` connects AndroidX `NavController` state to common application navigation surfaces.

The current Droid wrapper exposes static helpers for app bars, Material navigation views and bars, menu destination selection, and Up navigation.

## Configure an AppCompat action bar

Use an `AppBarConfiguration` together with the activity's `NavController`:

```swift
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
```

When handling the Up action, delegate through the same configuration:

```swift
let handled = NavigationUI.navigateUp(navController, configuration)
```

## Connect a Material NavigationView

`setupWithNavController` can keep a Material `NavigationView` in sync with the controller:

```swift
NavigationUI.setupWithNavController(navigationView, navController)
```

The wrapper also provides an overload for `NavigationBarView` and `onNavDestinationSelected(_:_:)` for menu-driven navigation.

## Current limitations

Droid does not currently expose the NavigationUI overloads that accept an AndroidX `Openable` or `CollapsingToolbarLayout`.

Use only the overloads explicitly exposed by the wrapper.
