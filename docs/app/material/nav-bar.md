# BottomNavigationView

`BottomNavigationView` is the Material bottom navigation bar: a persistent row of top-level destinations.

It works best for 3–5 sections users jump between all the time (Home / Search / Inbox / Profile). In Droid it inherits `NavigationBarView`, so menu, selection, label, and badge APIs come from that base.

## Use it when…

Use it when the app has a small number of stable top-level destinations and navigation should stay visible.

Use it when a drawer would be overkill and switching sections should be one tap away.

## How it behaves

Items come from a `Menu`. Inflate a menu resource with `inflateMenu(_:)` or build items programmatically.

`selectedItemId(_:)` selects a destination and `selectedItemId()` reads the current one. Item selection listeners are not wrapped yet — drive selection from your own navigation state.

Label visibility, icon/text appearance, active-indicator chrome, and item tint are configurable on the inherited `NavigationBarView` surface. `BottomNavigationView` adds `itemHorizontalTranslationEnabled(_:)`.

Badges attach per menu item id through `getOrCreateBadge(at:)` / `removeBadge(at:)`. See [`BadgeDrawable`](badges.md).

## Examples

### Three destinations with an inbox badge

```swift
final class MainActivity: AppCompatActivity {
    private let bottomNav = BottomNavigationView()

    override func buildUI() {
        super.buildUI()
        body {
            LinearLayout {
                FrameLayout()
                    .height(0)
                    .weight(1)

                bottomNav
                    .width(.matchParent)
                    .height(.wrapContent)
                    .itemHorizontalTranslationEnabled(false)
                    .labelVisibilityMode(.labeled)
            }
            .orientation(.vertical)
        }
    }

    override func onStart() {
        super.onStart()

        guard let menu = bottomNav.menu() else { return }
        menu.clear()
        menu.add(itemId: 1, title: "Home")
        menu.add(itemId: 2, title: "Search")
        menu.add(itemId: 3, title: "Inbox")

        bottomNav.selectedItemId(1)

        bottomNav.getOrCreateBadge(at: 3)?
            .number(4)
            .visible(true)
    }
}
```

### Jump to a destination from code

```swift
MaterialButton("Go to Inbox")
    .onClick { bottomNav.selectedItemId(3) }
```

## Methods

### Selection and menu

```swift
bottomNav.selectedItemId(2)
let current = bottomNav.selectedItemId()

bottomNav.inflateMenu(R.menu.main)
if let menu = bottomNav.menu() {
    // inspect or mutate items
}
```

`selectedItemId()`, `menu()`, `menuViewGroup()`, `maxItemCount()`, and `isSubMenuSupported()` read live state.

### Badges

```swift
bottomNav.getOrCreateBadge(at: 3)?.number(12).visible(true)
bottomNav.badge(at: 3)
bottomNav.removeBadge(at: 3)
```

Badge helpers return `BadgeDrawable?`. See [`BadgeDrawable`](badges.md) for content and placement.

### Labels and icons

```swift
bottomNav.labelVisibilityMode(.labeled)
bottomNav.labelMaxLines(1)
bottomNav.itemIconSize(24)
bottomNav.itemIconTintList(tint)
bottomNav.itemTextColor(tint)
```

`labelFontScalingEnabled(_:)`, `scaleLabelTextWithFont()`, and horizontal item text appearance resource setters refine label chrome. `itemIconGravity(_:)` / `itemGravity(_:)` control item layout gravity.

### Active indicator

```swift
bottomNav.itemActiveIndicatorEnabled(true)
bottomNav.itemActiveIndicatorHeight(32)
bottomNav.itemActiveIndicatorWidth(.wrapContent) // or .matchParent
```

Expanded width/height/margin helpers and `itemActiveIndicatorColor(_:)` shape the selection pill. `getItemActiveIndicatorColor()` / `isItemActiveIndicatorEnabled()` read live values.

### Item chrome

```swift
bottomNav.itemBackgroundResource(R.drawable.nav_item_bg)
bottomNav.itemRippleColor(ripple)
bottomNav.itemPaddingTop(4)
bottomNav.itemPaddingBottom(4)
```

Text appearance setters exist for active/inactive, including a bold-active toggle.

### BottomNavigationView extras

```swift
bottomNav.itemHorizontalTranslationEnabled(false)
bottomNav.isItemHorizontalTranslationEnabled()
bottomNav.clearOnItemSelectedListener()
bottomNav.clearOnItemReselectedListener()
```

`onTouchEvent(_:)` is available for low-level touch handling.

## Notes

Selection listeners (`setOnItemSelectedListener`) are not wrapped. Treat the bar as something you configure and drive from your own navigation state.

`BadgeUtils` attach/detach helpers remain available if you host badges outside `getOrCreateBadge`.
