# TextSwitcher

`TextSwitcher` specializes `ViewSwitcher` for animated text changes. It inherits the Android view-factory bridge and `ViewAnimator` transition controls, and adds live methods for replacing the currently displayed text or switching to new text.

## Use it when…

Use `TextSwitcher` when you specifically want Android's two-view text-switching behavior, such as animated counters, status labels, or changing headlines.

For ordinary state-driven text without this switching container, a `TextView` is simpler.

## How it behaves

`currentText(_:)` and `text(_:)` are immediate live-instance actions in the current wrapper; they are not queued builder properties.

The inherited `factory(_:)`, displayed-child, and animation configuration remain available.

## Examples

### Configure transition behavior

```swift
TextSwitcher()
    .animateFirstView(false)
```

### Change text on a mounted switcher

```swift
let switcher = TextSwitcher()

switcher.currentText("Ready")
switcher.text("Running")
```

Both text methods require a live Android instance.

## Initializers

```swift
TextSwitcher()
TextSwitcher(id: 1001)
```

## Methods

### Current text

`currentText(_:)` calls Android's `setCurrentText` on the live instance. It updates the text currently being shown without switching views.

### Switch to new text

`text(_:)` calls Android's `setText` on the live instance. Android sets the text on the next view and switches to it.

### Inherited factory and animation controls

`TextSwitcher` inherits `factory(_:)` from `ViewSwitcher`, plus `displayedChild(_:)`, `showNext()`, `showPrevious()`, `animateFirstView(_:)`, and enter/exit animation configuration from `ViewAnimator`.

The factory accepts a JNI-compatible Android object; Droid does not construct it for you.
