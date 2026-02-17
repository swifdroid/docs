# SeekBar

`SeekBar` is a draggable slider for choosing a value within a range, such as volume, brightness, playback position, or filter strength.

The current Droid type inherits working range, thumb, tick-mark, keyboard-step, and live progress APIs from the progress/seek base classes. Its own legacy `progress(...)` and listener methods are still present as unwired stubs, so avoid those overloads for now.

## Use it when…

Use `SeekBar` when dragging is faster and more natural than typing an exact value. Pair it with a label when users need to see the current number while they scrub.

## How it behaves

Range and track configuration such as `min(_:)`, `max(_:)`, `keyProgressIncrement(_:)`, `splitTrack(_:)`, and thumb/tick styling can be applied while building the control.

To change the value on a live `SeekBar`, use the inherited `setProgress(_:)` method. Do not use `SeekBar.progress(_:)` or `progress(_:animated:)` in the current source snapshot: those two overloads are still no-op stubs.

## Examples

### Configure a bounded slider

```swift
SeekBar()
    .min(0)
    .max(100)
    .keyProgressIncrement(5)
```

### Update a live slider

```swift
func updateVolume(_ seekBar: SeekBar, value: Int) {
    seekBar.setProgress(value)
}
```

`setProgress(_:)` comes from the working progress-bar surface and updates the current Android instance.

## Initializers

```swift
SeekBar()
SeekBar(id: 1001)
```

## Methods

### Range and progress

`min(_:)` and `max(_:)` configure the allowed range before attachment. On a live control, `getMin()`, `getMax()`, `getProgress()`, and `setProgress(_:)` read or update the current Android values.

```swift
seekBar.setProgress(50)
let current = seekBar.getProgress()
```

### Thumb, tick marks, and keyboard steps

`thumbOffset(_:)`, `thumbTintList(_:)`, `tickMarkTintList(_:)`, `splitTrack(_:)`, and `keyProgressIncrement(_:)` are available through the seek-style base behavior.

```swift
SeekBar()
    .thumbOffset(8)
    .splitTrack(true)
    .keyProgressIncrement(1)
```

### Current limitations

`progress(_:)`, `progress(_:animated:)`, and `onSeekBarChangeListener(_:)` are declared directly on `SeekBar` but their JNI calls are currently commented out, so they do not perform Android work.

`onInitializeAccessibilityNodeInfoInternal(_:)` is wired for low-level accessibility integration, but normal application code usually does not call it directly.
