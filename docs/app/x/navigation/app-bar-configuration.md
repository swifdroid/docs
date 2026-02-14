# AppBarConfiguration

`AppBarConfiguration` configures how AndroidX Navigation UI treats top-level destinations when it manages an app bar.

Use it together with `NavigationUI` when the app bar should hide the Up button on selected destinations and use AndroidX navigation semantics elsewhere.

## Build a configuration

Create a configuration from destination IDs that should be treated as top level:

```swift
func makeAppBarConfiguration() -> AppBarConfiguration? {
    AppBarConfiguration.Builder(100, 200)?.build()
}
```

You can also construct the builder from a `NavGraph` or a `Menu`.

## Check a destination

`isTopLevelDestination(_:)` tells you whether a destination belongs to the configured top-level set:

```swift
func isTopLevel(
    _ destination: NavDestination,
    in configuration: AppBarConfiguration
) -> Bool {
    configuration.isTopLevelDestination(destination)
}
```

## Current limitations

Droid does not currently expose the fallback navigate-up listener, openable-layout configuration, or the top-level destination set as readable properties.

The builder also does not currently expose fallback-listener or openable-layout setters.
