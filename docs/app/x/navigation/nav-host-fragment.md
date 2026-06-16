# NavHostFragment

`NavHostFragment` gives one fragment area its own navigation graph. Place it in an existing [`FragmentContainerView`](../fragment-container-view.md), then use its controller to move between destinations in that graph.

Create the host with the graph resource ID and arguments for its start destination. Creating the fragment does not install it in the activity. A fragment transaction does that separately, and its commit is scheduled work rather than an immediate display guarantee.

## Use it when…

Use a `NavHostFragment` when part of a screen needs its own fragment navigation flow. It keeps that flow behind a fragment container while the rest of the activity can remain in place.

## How it behaves

The `create` factory returns an optional host and accepts a `Contextable` value along with the graph ID and start-destination `Bundle`. The container ID you pass to the transaction must already identify a `FragmentContainerView` in the activity layout.

`supportFragmentManager()`, `beginTransaction()`, `replace(...)`, and `commit()` can each fail. Check their results before continuing. `commit()` schedules the transaction on the main thread; it does not mean the host is attached or visible yet. Commit before the activity saves its state, or the transaction can fail and its state may be lost.

The host's `navController()` can remain `nil` until `onCreate` has run and the host has had an opportunity to restore saved state. Keep controller use separate from host creation and call it only when the host is ready. These navigation APIs are main-actor isolated.

## Examples

### Schedule a host in an existing container

```swift
@MainActor
func scheduleNavigationHost(
    in activity: FragmentActivity,
    containerViewId: Int32,
    graphResourceId: Int32,
    startArguments: Bundle,
    context: Contextable
) -> NavHostFragment? {
    guard let host = NavHostFragment.create(
        graphResourceId,
        startArguments,
        context
    ) else {
        return nil
    }

    guard
        let manager = activity.supportFragmentManager(),
        let transaction = manager.beginTransaction(),
        let replacement = transaction.replace(
            containerViewId: containerViewId,
            fragment: host
        ),
        let _ = replacement.commit()
    else {
        return nil
    }

    return host
}
```

The caller supplies the existing container ID and graph inputs. A returned host means the wrapper calls produced values and the commit was scheduled; it does not confirm attachment or display.

### Navigate after the host is ready

```swift
@MainActor
func showDestination(
    _ destinationId: Int32,
    in host: NavHostFragment
) {
    guard let controller = host.navController() else { return }
    controller.navigate(destinationId)
}
```

Pass an ID for an action or destination in the host's current graph. `navigate(_:)` returns no result, so this call does not report whether a destination appeared on screen.

## Initializers

Use `create` for a graph-backed application host. The required initializer takes an existing `JObject`. The no-argument IUO initializer and the environment/class-name initializer can fail and should be checked when used by low-level integration code.

The convenience initializer that accepts a `JClassName` ignores that argument and uses `NavHostFragment`'s own native class name; it does not select a different fragment class. Application code should obtain controllers from a host or the `Navigation` lookup helpers rather than construct a `NavHostController` directly.

## Methods

### Get a controller from a fragment

Use `navController()` when you already hold the host. It stays optional until the host has completed `onCreate` and had a chance to restore saved state:

```swift
@MainActor
func controller(for host: NavHostFragment) -> NavController? {
    host.navController()
}
```

For a destination fragment, the static lookup searches its parent-fragment chain and then its view hierarchy:

```swift
@MainActor
func controller(for fragment: Fragment) -> NavController? {
    NavHostFragment.findNavController(fragment)
}
```

### Find a controller from a view

`Navigation` is created from an `ActivityContext` with a failable initializer. Its view lookup is an instance method and returns an optional controller:

```swift
@MainActor
func controller(
    for view: View,
    in context: ActivityContext
) -> NavController? {
    guard let navigation = Navigation(context) else { return nil }
    return navigation.findNavController(view)
}
```

When you have the host view's ID instead, use the overload that also takes its containing activity context:

```swift
@MainActor
func controller(
    for viewId: Int32,
    in context: ActivityContext
) -> NavController? {
    guard let navigation = Navigation(context) else { return nil }
    return navigation.findNavController(context, viewId)
}
```

The view overload also returns `nil` if the supplied `View` has no Android instance. Both lookups return `nil` when they cannot obtain a controller. See [`NavController`](nav-controller.md) for the operations available after lookup.
