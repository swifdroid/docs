# TwoLineListItem

`TwoLineListItem` is Android’s classic two-line list row.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; primary/secondary text accessors and row-mode APIs are not wrapped yet.

## Use it when…

Use `TwoLineListItem` when the screen needs the classic Android two-line row name and those APIs can land later.

## How it behaves

`TwoLineListItem` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit `RelativeLayout` and does not expose `getText1()` / `getText2()` or mode setters at this pin.

## Examples

### Place a two-line row

```swift
TwoLineListItem(id: 3801)
    .padding(h: 16, v: 12)
```

## Initializers

```swift
TwoLineListItem()
TwoLineListItem(id: 3801)
```

## Notes

Android’s `TwoLineListItem` binds two `TextView` children for list rows. That surface is not part of the current Droid API.
