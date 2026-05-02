# SeekBar

`SeekBar` is a draggable slider for choosing a value in a range, such as volume, brightness, or filter strength.

In the current Droid source pin it inherits [`AbsSeekBar`](abs-seek-bar.md) and [`ProgressBar`](progress-bar.md). Shared view interaction and layout APIs work. The declared `progress(...)` overloads and `onSeekBarChangeListener(_:)` exist as method names but are not wired to Android at this pin — do not rely on them.

## Use it when…

Use `SeekBar` when dragging is faster than typing an exact value and a slider communicates the range clearly.

For stars, use [`RatingBar`](rating-bar.md). For a non-interactive meter, use [`ProgressBar`](progress-bar.md).

## How it behaves

`SeekBar` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

Range, thumb, tick, and live progress APIs from the Android seek family are **not** wrapped yet. In particular, `min(_:)` / `max(_:)` / `setProgress(_:)` are not available on this Droid type.

## Examples

### Place a slider

```swift
SeekBar(id: 1601)
    .padding(h: 16, v: 12)
```

### Disable scrubbing until media is ready

```swift
SeekBar(id: 1602)
    .enabled(false)
```

## Initializers

```swift
SeekBar()
SeekBar(id: 1601)
```

## Notes

`onSeekBarChangeListener(_:)`, `progress(_:)`, and `progress(_:animated:)` are present in the Swift surface at this pin but their Android calls are not implemented. Treat them as unavailable until a later pin wires them.

Do not document or use `SeekBarOnSeekBarChangeListener` as a working callback bridge yet.
