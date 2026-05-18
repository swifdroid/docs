# NavigationView

`NavigationView` is the menu surface you put inside a drawer or side sheet: the place where “Home”, “Settings”, “Help”, and similar destinations live.

In Droid it owns a `Menu`, renders checked state, and exposes Material spacing, inset, and header configuration. Keep a reference so you can populate the menu after the view is attached.

## Use it when…

Use it when the app has many destinations, or when switching sections is not something users do every few seconds.

Use it when you want a consistent place for secondary screens: Settings, About, Feedback, legal links.

## How it behaves

`NavigationView` renders a vertical list of menu items. One item can be checked (`checkedItem(_:)` / `checkedItem()`), which helps users see where they are.

It is usually hosted by a drawer container such as `DrawerLayout`, or used as a static side panel in a two-pane layout. Header views, divider insets, item padding, and scrim inset flags are configurable. Navigation item-selected listeners are not wrapped yet.

## Examples

### Static side navigation

```swift
final class TwoPaneActivity: AppCompatActivity {
    private let nav = NavigationView()

    override func buildUI() {
        super.buildUI()
        body {
            LinearLayout {
                nav
                    .width(280)
                    .height(.matchParent)
                    .itemHorizontalPadding(16)
                    .itemIconPadding(12)

                FrameLayout()
                    .width(0)
                    .height(.matchParent)
                    .weight(1)
            }
            .orientation(.horizontal)
        }
    }

    override func onStart() {
        super.onStart()

        guard let menu = nav.menu() else { return }
        menu.clear()
        let home = menu.add(itemId: 1, title: "Home")
        _ = menu.add(itemId: 2, title: "Downloads")
        _ = menu.add(itemId: 3, title: "Settings")

        if let home {
            nav.checkedItem(home)
        }
    }
}
```

### Header and inset tuning

```swift
let header = TextView("Account").padding(16)

NavigationView()
    .addHeaderView(header)
    .dividerInsetStart(16)
    .dividerInsetEnd(16)
    .itemVerticalPadding(8)
    .startInsetScrimEnabled(false)
```

## Methods

### Menu and checked item

```swift
guard let menu = nav.menu() else { return }
menu.add(itemId: 1, title: "Home")

nav.checkedItem(item)
let current = nav.checkedItem()
```

### Headers

```swift
nav.addHeaderView(headerView)
nav.removeHeaderView(headerView)
let count = nav.headerCount()
let first = nav.headerView(at: 0)
```

### Item appearance

```swift
nav.itemHorizontalPadding(16)
nav.itemIconPadding(12)
nav.itemVerticalPadding(8)
nav.itemMaxLines(1)
nav.itemTextAppearance(R.style.NavItem)
nav.itemTextAppearanceActiveBoldEnabled(true)
nav.itemIconTintList(tint)
nav.itemTextColor(tint)
nav.itemBackgroundResource(R.drawable.nav_item_bg)
```

Resource overloads exist for horizontal/icon/vertical padding.

### Insets and dividers

```swift
nav.dividerInsetStart(16)
nav.dividerInsetEnd(16)
nav.subheaderInsetStart(8)
nav.subheaderInsetEnd(8)
```

Scrim inset flags: `bottomInsetScrimEnabled(_:)`, `topInsetScrimEnabled(_:)`, `startInsetScrimEnabled(_:)`. The end side has only the live getter `isEndInsetScrimEnabled()` — there is no `endInsetScrimEnabled(_:)` setter at this pin.

Foreground helpers: `drawTopInsetForeground(_:)`, `drawBottomInsetForeground(_:)`, `drawLeftInsetForeground(_:)`, `drawRightInsetForeground(_:)`, and `scrimInsetForeground(_:)`.

Back-progress helpers `cancelBackProgress()` / `handleBackInvoked()` are present; the intermediate back-progress setters are not wrapped yet.

## Notes

`setNavigationItemSelectedListener` is not wrapped at this pin, so reacting to taps is limited. Drive checked state from your own navigation logic.

`itemIconSize(_:)` is available for icon sizing. Do not assume Android’s full `NavigationView` listener surface.
