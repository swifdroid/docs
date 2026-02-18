# RatingBar

`RatingBar` is the classic star-rating control. It exposes the current rating, number of stars, step size, indicator mode, and the shared seek-style behavior inherited from `AbsSeekBar`.

Use it when the value is genuinely a rating rather than a generic numeric range.

## Use it when…

Use `RatingBar` for product, content, service, or experience ratings where stars communicate the scale clearly. For an arbitrary numeric value, [SeekBar](seek-bar.md) is usually a better fit.

## How it behaves

`numStars(_:)`, `rating(_:)`, `stepSize(_:)`, and `isIndicator(_:)` are Droid view properties. They can be configured declaratively while the view is being built.

Indicator mode keeps the visual rating but prevents normal user editing. The getter methods read the current Android instance.

## Examples

### Collect a half-star rating

```swift
RatingBar()
    .numStars(5)
    .stepSize(0.5)
    .rating(3.5)
```

### Show a read-only score

```swift
RatingBar()
    .numStars(5)
    .rating(4)
    .isIndicator(true)
```

## Initializers

```swift
RatingBar()
RatingBar(id: 1001)
```

## Methods

### Rating value

`rating(_:)` changes the current rating, while `rating()` reads it from the live view.

```swift
ratingBar.rating(4.5)
let value = ratingBar.rating()
```

### Stars and step size

`numStars(_:)` controls how many stars are displayed. `stepSize(_:)` controls the smallest rating increment.

```swift
ratingBar
    .numStars(5)
    .stepSize(1)
```

Use `numStars()` and `stepSize()` when you need the current values from a live instance.

### Indicator mode

`isIndicator(true)` makes the rating display-only. `isIndicator()` reports the current mode.

```swift
ratingBar.isIndicator(true)
```
