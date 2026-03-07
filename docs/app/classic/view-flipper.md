# ViewFlipper

`ViewFlipper` adds timed automatic cycling to Droid's `ViewAnimator` surface. It inherits displayed-child selection, enter/exit animations, child management, and manual next/previous switching.

## Use it when…

Use `ViewFlipper` when Android should rotate through a small set of child views on a timer, such as a simple status carousel or legacy rotating panel.

## How it behaves

`flipInterval(_:)` and `autoStart(_:)` are queued Droid view properties.

`startFlipping()`, `stopFlipping()`, and lifecycle helpers operate on the live Android instance.

All `ViewAnimator` switching and animation APIs remain available through inheritance.

## Examples

### Configure automatic flipping

```swift
ViewFlipper()
    .flipInterval(3_000)
    .autoStart(true)
    .animateFirstView(false)
```

### Start and stop manually

```swift
let flipper = ViewFlipper()

flipper.startFlipping()
flipper.stopFlipping()
```

The start/stop methods require a live Android instance.

## Initializers

```swift
ViewFlipper()
ViewFlipper(id: 1001)

ViewFlipper {
    TextView("First")
}
```

## Methods

### Flip interval

`flipInterval(_:)` queues the delay between automatic flips in milliseconds.

`flipInterval()` reads the current live interval.

### Auto-start

`autoStart(_:)` queues whether Android should automatically start flipping when attached to a window.

`autoStart()` and `isAutoStart()` read the current live setting.

### Flipping state

`startFlipping()` starts timed cycling on the live instance.

`stopFlipping()` stops it.

`isFlipping()` reports whether the live widget is currently flipping.

### Inherited animator controls

Because `ViewFlipper` inherits `ViewAnimator`, you can also use `displayedChild(_:)`, `showNext()`, `showPrevious()`, enter/exit animation configuration, and child-management helpers.

### Lower-level lifecycle helpers

The wrapper also exposes attach/detach and window-visibility callbacks for lower-level integrations.
