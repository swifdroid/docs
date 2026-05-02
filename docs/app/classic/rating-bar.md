# RatingBar

`RatingBar` is the classic star-rating control.

In the current Droid source pin it inherits [`AbsSeekBar`](abs-seek-bar.md) and [`ProgressBar`](progress-bar.md). Shared view interaction and layout APIs work; rating, star count, step size, and indicator mode are not wrapped yet.

## Use it when…

Use `RatingBar` when the value is genuinely a rating and stars communicate the scale better than a plain number.

For an arbitrary numeric range, use [`SeekBar`](seek-bar.md).

## How it behaves

`RatingBar` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `rating(_:)`, `numStars(_:)`, `stepSize(_:)`, `isIndicator(_:)`, or rating-change listeners at this pin.

## Examples

### Place a rating control

```swift
RatingBar(id: 1701)
    .padding(h: 16, v: 12)
```

### Show a read-only rating slot

```swift
RatingBar(id: 1702)
    .enabled(false)
```

## Initializers

```swift
RatingBar()
RatingBar(id: 1701)
```

## Notes

Android’s `RatingBar` owns star count, step size, and indicator mode. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
