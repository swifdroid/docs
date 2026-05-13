# FloatingActionButton

`FloatingActionButton` (FAB) is the circular, promoted action that floats above your content.
It’s at its best when there’s one obvious “primary” action: compose, add, scan, create.

In Droid it’s a normal view you place in your layout — most commonly inside a `CoordinatorLayout` so it can cooperate with scrolling and snackbars.

## Use it when…

Use it when the screen has a single high-priority action that should stay reachable while the user scrolls.

Use it when you want an icon-only action button that feels native to Material.

## How it behaves

A FAB draws a circular background and an icon, and it can animate when shown/hidden.
You can pick a standard size (`.normal`, `.mini`) or set a custom size.

On some devices/platform versions, the FAB can add internal padding (`useCompatPadding`) so it keeps consistent shadows and touch area.

## Examples

### A “compose” FAB above a scrolling list

```swift
import Droid

final class MessagesActivity: AppCompatActivity {
    override func buildUI() {
        super.buildUI()
        body {
            CoordinatorLayout {
                NestedScrollView {
                    LinearLayout {
                        ForEach(0..<30) { i in
                            TextView("Message \(i)")
                                .padding(16)
                        }
                    }
                    .orientation(.vertical)
                }
                .width(.matchParent)
                .height(.matchParent)

                FloatingActionButton()
                    .imageResource(R().drawable.ic_add)
                    .size(.normal)
                    .useCompatPadding()
                    .gravityInParent([.bottom, .end])
                    .margin(16)
                    .onClick {
                        Snackbar.make(contentView!, "Compose tapped", .short)?.show()
                    }
            }
        }
    }
}
```

### A mini FAB as a secondary action

```swift
body {
    FrameLayout {
        TextView("Map goes here")
            .width(.matchParent)
            .height(.matchParent)
            .gravity(.center)

        FloatingActionButton()
            .imageResource(R().drawable.ic_my_location)
            .size(.mini)
            .gravityInParent([.bottom, .start])
            .margin(16)
    }
}
```

## Methods

### Icon

```swift
FloatingActionButton()
    .imageResource(R().drawable.ic_add)

// or a Drawable instance
fab.imageDrawable(drawable)
```

`maxImageSize(_:)` caps the icon size.

### Size and touch target

```swift
FloatingActionButton()
    .size(.mini) // or .normal
    .customSize(56)
    .ensureMinTouchTargetSize(true)
    .useCompatPadding(true)
    .clearCustomSize()
```

### Elevation and motion

```swift
fab.compatElevation(6)
fab.compatHoveredFocusedTranslationZ(8)
fab.compatPressedTranslationZ(12)
fab.showMotionSpecResource(R.animator.fab_show)
fab.hideMotionSpecResource(R.animator.fab_hide)
```

Resource overloads exist for the elevation family.

### Show and hide

`show()` and `hide()` are imperative (they run an animation if the view is laid out), so call them in response to events.

```swift
fab.hide()
fab.show()
```

`isExpanded()`, `isOrWillBeShown()`, and `isOrWillBeHidden()` read live visibility state. `expanded(_:)` / `expandedComponentIdHint(_:)` relate to extended-FAB expansion.

### Appearance extras

```swift
fab.rippleColor(color)
fab.gravity([.bottom, .end])
```

`shapeAppearanceModel(_:)` accepts a `ShapeAppearanceModel` (corner sizing via `withCornerSize(_:)`). That helper type is support-only — no separate page.

## Notes

If you want the FAB to move out of the way of a `Snackbar`, put both in a `CoordinatorLayout`.

Extended FAB APIs are not wrapped on this type at the current pin. `internalSetVisibility` / `onTouchEvent` / `jumpDrawablesToCurrentState` exist for low-level integrations.
