# ProgressBar

`ProgressBar` shows work that is measurable or still open-ended.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; progress values, min/max bounds, and indeterminate mode are not wrapped yet.

## Use it when…

Use a progress bar when a screen should show that work is happening. A determinate bar is the usual choice once the operation can report a fraction; an indeterminate bar fits open-ended waits.

## How it behaves

`ProgressBar` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `setProgress(_:)`, `max(_:)`, `min(_:)`, `secondaryProgress(_:)`, `indeterminate(_:)`, or incremental update helpers at this pin.

## Examples

### Place a progress indicator

```swift
ProgressBar(id: 1401)
    .padding(h: 16, v: 12)
```

### Hide it when the operation finishes

```swift
let bar = ProgressBar(id: 1402)

// while working
bar.visibility(.visible)

// when done
bar.visibility(.gone)
```

## Initializers

```swift
ProgressBar()
ProgressBar(id: 1401)
```

## Notes

Android’s `ProgressBar` owns determinate/indeterminate state and numeric bounds. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
