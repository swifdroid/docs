# MaterialButton

`MaterialButton` is the Material-styled button: it matches your Material theme out of the box.

In Droid it inherits the text and click surface from `Button` / `TextView`. Its own methods at this pin are mostly getters for corner radius, icon placement, and insets — configure look through the theme.

## Use it when…

Use it when the action should look like part of a Material design system: primary/secondary actions, dialogs, forms, and call-to-action rows.

Use it when shape, ripple, and typography should come from the theme instead of per-screen tweaks.

## How it behaves

A `MaterialButton` draws and owns its Material background. Label text comes from the inherited `TextView` / `SetTextable` surface (`text(_:)`, or a string/state initializer).

`onClick(_:)` handles taps like any other view. Icon, corner, and inset **setters** are not wrapped at this pin — only the matching getters exist for inspection after layout.

## Examples

### Form submit

```swift
MaterialButton("Sign in")
    .width(.matchParent)
    .onClick { status = "Signed in" }
    .padding(left: 16, top: 0, right: 16, bottom: 8)
```

### Two actions side by side

```swift
body {
    LinearLayout {
        MaterialButton("Cancel")
            .width(0)
            .weight(1)
            .marginEnd(8)

        MaterialButton("Save")
            .width(0)
            .weight(1)
    }
    .orientation(.horizontal)
    .padding(16)
}
```

### Inspect theme-derived geometry

```swift
let radius = button.cornerRadius()
let iconMode = button.iconGravity()
let topInset = button.insetTop()
```

## Methods

### Material getters

```swift
button.cornerRadius()
button.icon()
button.iconGravity()
button.iconPadding()
button.iconSize()
button.insetTop()
button.insetBottom()
button.clearOnCheckedChangeListeners()
```

`IconGravity` cases: `.start`, `.end`, `.textStart`, `.textEnd`, `.top`, `.textTop`.

`clearOnCheckedChangeListeners()` removes previously registered checked-change listeners (listener *registration* is not wrapped yet).

### Inherited text and click

```swift
MaterialButton("Save")
    .textSize(16)
    .maxLines(1)
    .onClick { /* handle */ }
```

Text, padding, visibility, and click behavior come from [`TextView`](../../classic/text-view.md) and the shared view API.

## Notes

There are no fluent setters for `icon(_:)`, `cornerRadius(_:)`, `iconPadding(_:)`, `iconSize(_:)`, `insetTop(_:)`, `insetBottom(_:)`, or `iconGravity(_:)` at this pin. XML `app:icon` / `app:cornerRadius` attributes are not Swift API.

Also not wrapped yet: `addOnCheckedChangeListener`, `getBackgroundTintList`, `getBackgroundTintMode`. Style those from the Material theme.

`AppCompatButton` inheritance is thin at this pin (emoji-compat helpers only) plus classic `Button` → `TextView`.
