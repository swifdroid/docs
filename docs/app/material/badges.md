# BadgeDrawable

A badge is the tiny dot or number that sits on top of an icon: “3”, “99+”, “new”.

In Droid it is a drawable you create, configure, and attach to an existing view with `BadgeUtils`. Navigation bars can also own badges through `getOrCreateBadge(at:)`.

## Use it when…

Use it when a piece of UI needs a subtle “there’s something here” signal: unread count, pending requests, a new-feature dot.

Use it when the user should keep context on the current screen while still noticing that something changed elsewhere.

## How it behaves

A badge lives relative to an anchor view. When attached, it draws in the anchor’s overlay (or a custom parent’s foreground), so it visually sits on top of the anchor.

Badges can show either a number (`number(_:)`) or a short text label (`text(_:)`). If the content is longer than `maxCharacterCount(_:)`, Android truncates it to keep the badge readable. `clearNumber()` / `clearText()` remove the content again.

Gravity, offsets, padding, text color, and visibility are live drawable configuration. Attach only after the anchor has an Android instance.

## Examples

### Badge on a notification icon

```swift
final class InboxActivity: AppCompatActivity {
    private let bell = TextView("Notifications").textSize(18)
    private var badge: BadgeDrawable?

    override func buildUI() {
        super.buildUI()
        body {
            FrameLayout {
                bell.padding(16)
            }
        }
    }

    override func onStart() {
        super.onStart()
        guard let context else { return }

        if badge == nil {
            badge = BadgeDrawable.create(context)?
                .number(3)
                .visible(true)
        }

        guard let badge else { return }
        BadgeUtils.attachBadgeDrawable(badge, to: bell)
    }
}
```

### Badge on a navigation item

```swift
if let badge = bottomNav.getOrCreateBadge(at: 3) {
    badge.number(12).visible(true)
}

bottomNav.removeBadge(at: 2)
```

### Short text badge

```swift
badge.text("NEW")
    .maxCharacterCount(3)
    .badgeGravity([.top, .end])
```

## Methods

### Creating and attaching

`BadgeDrawable.create(_:)` needs an `ActivityContext` and returns `BadgeDrawable?`.

```swift
let badge = BadgeDrawable.create(context)
```

Attach and detach through `BadgeUtils`:

```swift
BadgeUtils.attachBadgeDrawable(badge, to: anchorView)
BadgeUtils.detachBadgeDrawable(badge, from: anchorView)
```

Toolbar menu items have parallel helpers: `attachBadgeDrawable(_:toMenuItem:in:in:)` and `detachBadgeDrawable(_:fromMenuItem:in:)`.

### Content

```swift
badge.number(7)
badge.clearNumber()

badge.text("NEW")
badge.clearText()
```

`number()` / `text()` read the live values. `hasNumber()` / `hasText()` report whether content is present.

### Visibility and placement

```swift
badge.visible(true)
badge.badgeGravity([.top, .end])
badge.maxCharacterCount(2)
badge.horizontalOffset(4)
badge.verticalOffset(2)
```

`badgeFixedEdge(_:)` pins the badge to `.start` or `.end` of the anchor. Offsets come in plain, `WithText`, and `WithoutText` variants so the badge can shift depending on content.

### Colors and appearance

`badgeTextColor(_:)` and `backgroundColor()` cover text and fill. `textAppearance(_:)` points at a style resource. Shape-appearance resource setters exist for with-text and without-text badges.

### Navigation-bar integration

`NavigationBarView.badge(at:)`, `getOrCreateBadge(at:)`, and `removeBadge(at:)` create or remove item badges without manual `BadgeUtils` calls.

`BadgeUtils.setBadgeDrawableBounds(_:for:in:)` and `updateBadgeBounds(_:_:_:_:_:_:)` (`Rect`, `centerX`, `centerY`, `halfWidth`, `halfHeight`, optional unit) are lower-level bounds helpers when you host the badge yourself.

## Notes

Attach in `onStart()` or later — before the anchor has an instance there is nothing to draw on.

`BadgeDrawable.createFromResource` and saved-state helpers are not wrapped at this pin. `badgeGravity()` getter is present but marked for verification in source; prefer the setter for configuration.
