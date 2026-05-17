# MaterialToolbar

`MaterialToolbar` is a top app bar that matches Material theming (elevation overlays, centered titles, etc.).
In Droid it behaves like a toolbar you configure — title, optional subtitle, and room for actions — rather than a generic “layout container”.

You’ll most often place it inside `AppBarLayout` (for scroll/collapse) or just at the top of a vertical layout for a simple screen.

## Use it when…

Use it when your screen needs a recognizable top bar: a title, optional subtitle, and space for actions.

Use it when you want Material styling without hand-tweaking elevation, overlays, and typography on every screen.

## How it behaves

The toolbar draws a background and lays out its title/subtitle and action area.
In Droid, `title(...)` and `subtitle(...)` accept `String` or `State<String>` — if you pass state, the toolbar updates live.

If you put the toolbar inside an `AppBarLayout`, you can give it `scrollFlags(...)` so it collapses and snaps with scrolling content.

## Examples

### Simple toolbar at the top of a screen

```swift
body {
    LinearLayout {
        MaterialToolbar()
            .title("Profile")
            .subtitle("Signed in")
            .width(.matchParent)
            .height(.wrapContent)

        TextView("Content goes here")
            .padding(16)
    }
    .orientation(.vertical)
}
```

### Toolbar inside an AppBarLayout (scrolling)

```swift
body {
    CoordinatorLayout {
        AppBarLayout {
            MaterialToolbar()
                .title("Inbox")
                .scrollFlags(.scroll, .enterAlways)
        }

        NestedScrollView {
            LinearLayout {
                ForEach(0..<30) { i in
                    TextView("Message \(i)")
                        .padding(16)
                }
            }
            .orientation(.vertical)
        }
        .behavior(AppBarLayout.ScrollingViewBehavior())
    }
}
```

### Title driven by state

```swift
@State var query = ""

body {
    MaterialToolbar()
        .title($query.map { $0.isEmpty ? "Search" : $0 })
}
```

## Methods

### Titles

`title(_:)` and `subtitle(_:)` accept `String` and `State<String>` (`_SetTitleable` / `_SetSubtitleable`):

```swift
MaterialToolbar()
    .title("Settings")
    .subtitle("Account")
```

### Menu refresh and overflow

Call `invalidateMenu()` when menu content changes and the toolbar should re-render.

```swift
toolbar.invalidateMenu()

let showing = toolbar.isOverflowMenuShowing()
let hid = toolbar.hideOverflowMenu()
let expanded = toolbar.hasExpandedActionView()
```

`isBackInvokedCallbackEnabled()` reports whether the predictive-back callback is enabled.

### Low-level event hooks

```swift
toolbar.onTouchEvent(event)
toolbar.onHoverEvent(event)
toolbar.onRtlPropertiesChanged(.ltr)
```

These forward to the live Android view for custom touch/hover/RTL handling.

## Notes

`removeMenuProvider` is not wrapped at this pin. Menu provider population across the framework is still growing — you can place a toolbar before a full Swift-first menu story exists.

`MaterialToolbar` is a `ViewGroup` with `init(id:)` and a single-child content initializer. Combined with `AppBarLayout` `scrollFlags` / `scrollEffect` helpers it can collapse with scrolling content.
