# ViewAnimator

`ViewAnimator` is Droid's classic one-child-at-a-time container. The current wrapper inherits `FrameLayout` and exposes displayed-child selection, enter/exit animations, first-view animation control, child management, and next/previous switching.

## Use it when…

Use `ViewAnimator` when several child views share the same space and Android should show only one at a time, optionally animating transitions.

## How it behaves

`displayedChild(_:)`, `animateFirstView(_:)`, `inAnimation(_:)`, and `outAnimation(_:)` are queued Droid view properties.

Getters plus `showNext()`, `showPrevious()`, and child-removal methods operate on the live Android instance.

## Examples

### Declare children and choose the initial one

```swift
ViewAnimator {
    TextView("First")
}
.displayedChild(0)
.animateFirstView(false)
```

### Move through children on a mounted animator

```swift
let animator = ViewAnimator()

animator.showNext()
animator.showPrevious()
```

The switching methods require a live Android instance.

## Initializers

```swift
ViewAnimator()
ViewAnimator(id: 1001)

ViewAnimator {
    TextView("Content")
}
```

## Methods

### Displayed child

`displayedChild(_:)` queues which child index Android should show.

`displayedChild()` reads the current live index, while `currentView()` returns the currently displayed `View?`.

### Animations

`inAnimation(_:)` and `outAnimation(_:)` queue optional Android `Animation` objects.

`inAnimation()` and `outAnimation()` read the live animation objects.

`animateFirstView(_:)` controls whether the first displayed child is animated; `animateFirstView()` reads the live setting.

### Switching

`showNext()` and `showPrevious()` immediately switch the live animator.

### Child management

The wrapper exposes `addView(_:_:_:)`, `removeAllViews()`, `removeView(_:)`, `removeViewAt(_:)`, and range/layout removal helpers for live child management.

### Baseline

`baseline()` reads the baseline of the currently displayed child.
