# SlidingDrawer

`SlidingDrawer` is a legacy Android panel that stays collapsed behind a handle until the user opens it. Opening slides the content into view; closing slides it away again.

Droid exposes the drawer as a `View` you can build with a content block and then drive with live `open` / `close` / `toggle` calls. The native widget also tracks whether it is opened or currently moving, so application code can keep controls in sync with the panel.

Treat this component as a compatibility surface. Android deprecated `SlidingDrawer` because the interaction feels dated next to modern navigation drawers and bottom sheets. Prefer those patterns for new screens; keep `SlidingDrawer` only when a legacy design must keep its old open/close behavior.

## Use it when…

Use `SlidingDrawer` on a legacy screen that already depends on a handle-plus-panel metaphor and must keep that exact interaction after migrating to Droid.

Do not choose it for new navigation. For app-level navigation, use a modern drawer (for example Material `NavigationView` inside a drawer container). For temporary detail content, a bottom sheet or dialog is usually a better fit.

## How it behaves

The drawer has two roles: a **handle** the user drags or taps, and **content** that appears when the drawer opens. After the native view is realized, `handle()` and `content()` return those child views when Android has assigned them.

`open()`, `close()`, and `toggle()` change state immediately. The `animate*` variants play the native open/close animation instead of jumping. `lock()` freezes interaction so the handle cannot change state; `unlock()` restores it.

`isOpened()` reports the current open state. `isMoving()` is true only while an open/close animation or drag is in flight. Use both when enabling buttons that would otherwise fight the animation.

Drawer listeners (`onDrawerOpenListener`, `onDrawerCloseListener`, `onDrawerScrollListener`) accept Android/JNI-compatible listener objects. They are live bridges: Droid does not wrap them in Swift closures.

## Examples

### Open and close a settings panel

```swift
let drawer = SlidingDrawer(id: 1001) {
    // handle
    LinearLayout {
        ImageView()
            .width(48)
            .height(48)
    }
    .width(.matchParent)
    .height(48)

    // content
    LinearLayout {
        TextView("Quick settings")
        Switch(id: 1101)
            .text("Airplane mode")
    }
    .orientation(.vertical)
    .width(.matchParent)
    .height(.matchParent)
    .padding(16)
}
.width(.matchParent)
.height(240)

drawer.open()
```

The panel slides out from behind the handle and shows the settings row. `drawer.close()` slides it away again.

### Toggle from a button and lock during load

```swift
func onHandleTap(_ drawer: SlidingDrawer) {
    guard !drawer.isMoving() else { return }
    drawer.animateToggle()
}

func refreshPanel(_ drawer: SlidingDrawer) {
    drawer.lock()
    // reload panel data here
    drawer.unlock()
}
```

`animateToggle()` keeps the native transition. `isMoving()` avoids stacking animations, and `lock()` prevents the user from opening the drawer while the panel reloads.

### Track open state after the drawer is realized

```swift
func syncChrome(_ drawer: SlidingDrawer, _ title: TextView) {
    if drawer.isOpened() {
        title.text("Details")
    } else {
        title.text("Tap the handle")
    }

    let handle = drawer.handle()
    let body = drawer.content()
}
```

Call these after Android has created the live drawer instance. `handle()` and `content()` return the live child views when they are available.

## Initializers

```swift
SlidingDrawer()

SlidingDrawer(id: 1001)

SlidingDrawer(id: 1001) {
    // handle and content views
}
```

Pass a stable id when other code needs to find the drawer later. The content block is the ordinary Droid view builder used to place the drawer’s child views.

## Methods

### Open, close, and toggle

`open()`, `close()`, and `toggle()` switch state immediately.

```swift
drawer.open()
drawer.toggle()
drawer.close()
```

Use the immediate forms when the UI must snap without animation.

### Animated open, close, and toggle

`animateOpen()`, `animateClose()`, and `animateToggle()` run the native drawer animation.

```swift
drawer.animateOpen()
drawer.animateToggle()
```

Prefer these for user-driven handle taps so the movement matches platform expectations.

### Lock and unlock

`lock()` disables drawer interaction. `unlock()` enables it again.

```swift
drawer.lock()
drawer.unlock()
```

Lock the drawer during short critical sections such as loading panel content.

### Read state and child views

`isOpened()` reports whether the drawer is open. `isMoving()` reports whether it is currently animating or being dragged. `handle()` and `content()` return the live handle and content views when Android has assigned them.

```swift
if drawer.isOpened() && !drawer.isMoving() {
    let panel = drawer.content()
}
```

### Drawer listeners

`onDrawerOpenListener(_:)`, `onDrawerCloseListener(_:)`, and `onDrawerScrollListener(_:)` attach Android-compatible listener objects for open, close, and scroll events.

```swift
drawer.onDrawerOpenListener(openListener)
drawer.onDrawerCloseListener(closeListener)
drawer.onDrawerScrollListener(scrollListener)
```

Each method accepts a JNI-compatible listener object. This surface does not take Swift closures.

## Notes

Android deprecated `SlidingDrawer`. Keep it only for legacy parity; new screens should use modern drawer or sheet patterns instead.

The current Droid surface reads `handle()` and `content()` after native assignment. It does not expose separate handle/content setters.
