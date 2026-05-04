# CheckedTextView

`CheckedTextView` is Android’s text row that can show a checked state.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; checked state, check-mark drawables, and `Checkable` methods are not wrapped yet.

## Use it when…

Use `CheckedTextView` when the screen needs the classic Android checked text row and those APIs can land later.

## How it behaves

`CheckedTextView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** inherit [`TextView`](text-view.md), does **not** conform to `Checkable`, and does not expose `checked(_:)`, `isChecked()`, `toggle()`, `checkMarkDrawable(_:)`, or `checkMarkTintList(_:)` at this pin.

## Examples

### Place a checked text row

```swift
CheckedTextView(id: 3401)
    .padding(h: 16, v: 12)
```

## Initializers

```swift
CheckedTextView()
CheckedTextView(id: 3401)
```

## Notes

Android’s `CheckedTextView` combines text with a check mark. That surface is not part of the current Droid API. Use [`CompoundButton`](compound-button.md) when you need verified `Checkable` methods.
