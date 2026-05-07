# QuickContactBadge

`QuickContactBadge` is Android’s contact-affordance badge.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; contact assignment, mode filtering, and badge image APIs are not wrapped yet.

## Use it when…

Use `QuickContactBadge` when the screen needs the classic Android contact badge and the contact APIs can land later.

## How it behaves

`QuickContactBadge` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose contact URI assignment, `setExcludeMimes(_:)`, `onClick(_:)` contact dispatch helpers, or badge drawable APIs at this pin.

## Examples

### Place a contact badge

```swift
QuickContactBadge(id: 2601)
    .padding(h: 8, v: 8)
```

## Initializers

```swift
QuickContactBadge()
QuickContactBadge(id: 2601)
```

## Notes

Android’s `QuickContactBadge` launches QuickContact for a `Contacts` URI. That behavior is not part of the current Droid surface.
