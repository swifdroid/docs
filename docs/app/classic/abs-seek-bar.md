# AbsSeekBar

`AbsSeekBar` is the shared Android base behind interactive progress controls such as [SeekBar](seek-bar.md) and [RatingBar](rating-bar.md). It adds the draggable thumb, tick marks, keyboard stepping, and track-specific configuration on top of `ProgressBar` behavior.

Android treats `AbsSeekBar` as an abstract base class, so normal app UI should create `SeekBar` or `RatingBar` rather than instantiate `AbsSeekBar` directly.

## Use it when…

Use this page when you need to understand behavior inherited by seek-style controls. For a draggable numeric slider, start with `SeekBar`; for stars, use `RatingBar`.

## How it behaves

Thumb, tick-mark, split-track, range, and key-step setters are Droid view properties, so they can be applied while building the control and then forwarded when the Android view exists.

Getter methods such as `thumb()`, `tickMark()`, and `thumbTintList()` read the live Android instance. Before attachment they can only return `nil` or fallback values.

## Examples

### Configure a stepped SeekBar

```swift
SeekBar()
    .min(0)
    .max(10)
    .keyProgressIncrement(1)
    .splitTrack(true)
```

### Tint and offset the thumb

```swift
func makeSeekBar(tint: ColorStateList) -> SeekBar {
    SeekBar()
        .thumbTintList(tint)
        .thumbOffset(8)
}
```

## Methods

### Thumb and track

`thumb(_:)` replaces the draggable thumb. `thumbTintList(_:)` applies its tint, and `thumbOffset(_:_:)` controls how far it can extend beyond the track.

```swift
seekBar.thumbOffset(12)
seekBar.splitTrack(true)
```

Read the current values from a live control with `thumb()`, `thumbTintList()`, `thumbOffset(_:)`, and `splitTrack()`.

### Tick marks

`tickMark(_:)` supplies the drawable used at progress positions, while `tickMarkTintList(_:)` tints those marks.

```swift
seekBar.tickMarkTintList(tint)
```

`tickMark()` and `tickMarkTintList()` read the currently installed drawable and tint.

### Range and keyboard stepping

`min(_:)` and `max(_:)` configure the seek range. `keyProgressIncrement(_:)` controls the amount changed by arrow-key input.

```swift
SeekBar()
    .min(0)
    .max(100)
    .keyProgressIncrement(5)
```

`keyProgressIncrement()` reads the current increment from a live instance.

### Low-level event helpers

`jumpDrawablesToCurrentState()`, `drawableHotspotChanged(x:y:)`, `onTouchEvent(_:)`, and `onKeyDown(_:_:)` are exposed for lower-level custom control work. Most application code should let the normal Android event pipeline call these.