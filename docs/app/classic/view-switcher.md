# ViewSwitcher

`ViewSwitcher` is Droid's two-view switching container. The current wrapper inherits `ViewAnimator`, exposes the next view, reset behavior, and a JNI-compatible Android `ViewFactory` bridge, while keeping the inherited animation and switching APIs.

## Use it when…

Use `ViewSwitcher` when Android should alternate between two lazily created views, such as read/edit states or two presentation modes.

## How it behaves

`factory(_:)` is a queued Droid view property.

`nextView()` and `reset()` operate on the live Android instance.

Inherited `ViewAnimator` properties such as displayed child and enter/exit animations remain available.

## Examples

### Configure inherited transition behavior

```swift
ViewSwitcher()
    .animateFirstView(false)
    .displayedChild(0)
```

### Inspect and reset a mounted switcher

```swift
let switcher = ViewSwitcher()

let next = switcher.nextView()
switcher.reset()
```

These live methods require a mounted Android instance.

## Initializers

```swift
ViewSwitcher()
ViewSwitcher(id: 1001)
```

The current wrapper does not expose a declarative child-content initializer.

## Methods

### View factory

`factory(_:)` accepts a JNI-compatible Android `ViewSwitcher.ViewFactory` object and queues it for the widget.

Droid exposes the factory bridge but does not construct the Android factory object for you.

### Next view

`nextView()` returns the next live `View?` that the switcher would display.

### Reset

`reset()` immediately resets the live switcher, hiding its existing views and clearing first-time animation state.

### Inherited animator controls

`ViewSwitcher` inherits `displayedChild(_:)`, `showNext()`, `showPrevious()`, `animateFirstView(_:)`, enter/exit animation configuration, and the `ViewAnimator` child-management helpers.
