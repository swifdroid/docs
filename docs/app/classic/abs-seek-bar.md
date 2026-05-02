# AbsSeekBar

`AbsSeekBar` is the shared Android base behind interactive progress controls such as [`SeekBar`](seek-bar.md) and [`RatingBar`](rating-bar.md).

Android treats `AbsSeekBar` as an abstract base. Prefer the concrete control for real UI. In the current Droid source pin this type only adds the inheritance chain above [`ProgressBar`](progress-bar.md); thumb, tick, and range APIs are not wrapped yet.

## Use it when…

Use this page to understand what the concrete seek-style controls currently inherit. For a draggable slider, start with [`SeekBar`](seek-bar.md). For stars, use [`RatingBar`](rating-bar.md).

## How it behaves

`AbsSeekBar` participates in the view hierarchy like other controls through its `ProgressBar` parent.

The current type does **not** expose `thumb(_:)`, `thumbTintList(_:)`, `tickMark(_:)`, `splitTrack(_:)`, `keyProgressIncrement(_:)`, or related seek APIs at this pin.

## Examples

Prefer the concrete controls:

```swift
SeekBar(id: 1501)
RatingBar(id: 1502)
```

## Initializers

```swift
AbsSeekBar()
AbsSeekBar(id: 1501)
```

## Notes

Thumb, tick, and range configuration live on Android’s `AbsSeekBar` API. None of that is part of the current Droid surface.
