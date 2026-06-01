# NavController

`NavController` manages navigation through an app's graph. Use an already-configured controller to inspect the current destination and graph, then navigate with directions supplied by your app.

For action-bar setup, Up handling, and menu selection, use [NavigationUI](navigation-ui.md). This guide focuses on inspecting navigation state and applying options to a direct navigation call.

## Use it when…

Use it when you need to read the graph's start destination or navigate to a detail screen with options that avoid another copy of the same destination at the top of the back stack.

## How it behaves

Call these APIs from the main actor. `currentDestination()` returns `NavDestination?`, and `graph()` returns `NavGraph?`; handle their optional results before using them. A graph exposes its start destination ID as an `Int32`.

Inspecting the graph does not replace it. Replacing a controller's graph replaces its existing navigation data, including the back stack, so graph replacement should be a deliberate setup change.

`NavOptions.Builder()` can fail, and `build()` returns `NavOptions?`. With `launchSingleTop(true)`, navigation requests single-top behavior: at most one copy of a given destination at the top of the back stack. This does not remove every earlier copy of that destination.

The directions-plus-options `navigate` call returns `Void`. It provides no Boolean result confirming that navigation succeeded.

## Examples

Both helpers receive a controller whose navigation setup is already complete. The navigation helper also requires existing `NavDirections` valid for that controller's graph and current destination.

Read the configured start destination ID without changing navigation state:

```swift
@MainActor
func currentDestination(in controller: NavController) -> NavDestination? {
    controller.currentDestination()
}

@MainActor
func startDestinationID(using controller: NavController) -> Int32? {
    guard let graph = controller.graph() else { return nil }
    return graph.startDestinationId()
}
```

Navigate to a detail destination using caller-provided directions and single-top options. If either construction step fails, the helper returns without making the navigation call:

```swift
@MainActor
func navigateToDetails(
    using controller: NavController,
    directions: NavDirections
) {
    guard let builder = NavOptions.Builder(),
          let options = builder.launchSingleTop(true).build()
    else { return }

    controller.navigate(directions, options)
}
```

## Methods

The short calls below belong in a main-actor context, with the controller and directions supplied by the app's navigation setup.

### Inspect the current destination and graph

Use `currentDestination()` to retrieve the optional current destination. Use `graph()` to retrieve the optional graph associated with the controller, then read its start destination ID:

```swift
@MainActor
func currentDestination(in controller: NavController) -> NavDestination? {
    controller.currentDestination()
}

@MainActor
func startDestinationID(using controller: NavController) -> Int32? {
    guard let graph = controller.graph() else { return nil }
    return graph.startDestinationId()
}
```

### Build navigation options

Call `launchSingleTop(_:)` on a successfully created builder to request single-top behavior. It returns the same builder for chaining. Call `build()` to obtain optional options, then unwrap them before navigation:

```swift
@MainActor
func makeSingleTopOptions() -> NavOptions? {
    guard let builder = NavOptions.Builder() else { return nil }
    return builder.launchSingleTop(true).build()
}
```

### Navigate with directions and options

Pass the existing directions and built options to `navigate(_:_:)`. Both arguments are required for this overload:

```swift
@MainActor
func navigateToDetails(
    using controller: NavController,
    directions: NavDirections,
    options: NavOptions
) {
    controller.navigate(directions, options)
}
```
