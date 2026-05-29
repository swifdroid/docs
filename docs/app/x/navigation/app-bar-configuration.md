# AppBarConfiguration

`AppBarConfiguration` holds the options that NavigationUI uses to configure app-bar behavior around top-level destinations. Choose which destinations should hide the Up button, then build a configuration to pass to NavigationUI.

[NavigationUI](navigation-ui.md) consumes the configuration when it sets up an app bar or handles Up navigation. Creating a configuration alone does not connect or update a toolbar.

## Use it when…

Use it when your graph's start destination should be the app's only top-level screen, or when several destinations—such as Home, Search, and Library—should each hide the Up button.

Use the menu-based builder when a navigation menu already identifies those top-level destinations.

## How it behaves

A graph-based builder uses the graph's start destination as its only top-level destination. An ID-based builder uses the supplied destination IDs. A menu-based builder uses the menu item IDs; each item ID is expected to correspond to a destination in the navigation graph.

The Up button is not shown on those top-level destinations when NavigationUI applies the configuration. `isTopLevelDestination(_:)` lets you query whether a `NavDestination` was directly configured as top level.

Builder construction can fail, and `build()` returns `AppBarConfiguration?`. Handle the optional result before passing it to NavigationUI.

## Examples

Build a configuration for a graph whose start destination should hide the Up button:

```swift
@MainActor
func makeAppBarConfiguration(for graph: NavGraph) -> AppBarConfiguration? {
    guard let builder = AppBarConfiguration.Builder(graph) else { return nil }
    return builder.build()
}
```

For several top-level screens, pass their actual destination IDs. This helper accepts the IDs assigned to Home, Search, and Library in your graph:

```swift
@MainActor
func makeTopLevelConfiguration(
    homeID: Int32,
    searchID: Int32,
    libraryID: Int32
) -> AppBarConfiguration? {
    AppBarConfiguration.Builder(homeID, searchID, libraryID)?.build()
}
```

## Initializers

Choose `AppBarConfiguration.Builder(graph)` for a single top-level start destination, as in the first example. Choose the variadic ID initializer above or the array initializer when you supply the top-level destinations explicitly:

```swift
@MainActor
func makeTopLevelConfiguration(ids: [Int32]) -> AppBarConfiguration? {
    AppBarConfiguration.Builder(ids)?.build()
}
```

Choose `AppBarConfiguration.Builder(menu)` when the menu item IDs match the graph's top-level destination IDs:

```swift
@MainActor
func makeMenuConfiguration(menu: Menu) -> AppBarConfiguration? {
    AppBarConfiguration.Builder(menu)?.build()
}
```

## Methods

### Build the configuration

Call `build()` on a successfully created builder to obtain an optional `AppBarConfiguration`:

```swift
@MainActor
func buildConfiguration(
    from builder: AppBarConfiguration.Builder
) -> AppBarConfiguration? {
    builder.build()
}
```

### Check top-level membership

Pass a `NavDestination` to `isTopLevelDestination(_:)` to check its configured membership. The result is a `Bool`:

```swift
@MainActor
func isTopLevel(
    _ destination: NavDestination,
    in configuration: AppBarConfiguration
) -> Bool {
    configuration.isTopLevelDestination(destination)
}
```

## Notes

Droid does not expose the fallback navigate-up listener, openable-layout getter, or top-level-destination getter. The Builder's fallback-listener and openable-layout setters are also not declared.

Use the nested Builder to construct a configuration for your app. `AppBarConfiguration.init(_ object: JObject)` wraps an existing JNI object.
