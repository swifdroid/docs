# ProgressBar

`ProgressBar` shows work that is either measurable or still open-ended. Use determinate progress when you know how much work is complete, and indeterminate mode when you only know that work is in progress.

The current Droid surface exposes live progress values, secondary progress, min/max bounds, indeterminate state, and incremental updates.

## Use it when…

Use a determinate bar for downloads, uploads, imports, or any operation where the UI can report a meaningful fraction. Use indeterminate mode for short waits where no useful percentage exists.

## How it behaves

`ProgressBar` setters such as `setProgress(_:)`, `setMax(_:)`, and `indeterminate(_:)` call the current Android view instance immediately. Apply them after the widget has been materialized rather than treating them as pre-attachment layout modifiers.

`secondaryProgress(_:)` gives you a second progress channel, which is useful for states such as buffered-versus-played media.

## Examples

### Update measurable progress

```swift
func updateDownload(
    _ progressBar: ProgressBar,
    completed: Int,
    total: Int
) {
    progressBar
        .setMin(0)
        .setMax(total)
        .setProgress(completed)
}
```

This flow assumes `progressBar` already represents a live Android view.

### Switch a live indicator into indeterminate mode

```swift
func showWaitingState(_ progressBar: ProgressBar) {
    progressBar.indeterminate(true)
}
```

## Initializers

```swift
ProgressBar()
ProgressBar(id: 1001)
```

## Methods

### Progress and range

`progress()` reads the current value. `progress(_:)` and `setProgress(_:)` update it. `getMin()`, `getMax()`, `setMin(_:)`, and `setMax(_:)` control the valid range.

```swift
let current = progressBar.progress()
let maximum = progressBar.getMax()
```

`incrementProgressBy(_:)` is useful when work arrives in discrete chunks:

```swift
progressBar.incrementProgressBy(1)
```

### Secondary progress

Use `secondaryProgress(_:)` when the UI needs a second measurable state:

```swift
progressBar.secondaryProgress(buffered)
```

`secondaryProgress()` reads the current secondary value, and `incrementSecondaryProgressBy(_:)` advances it incrementally.

### Indeterminate state

`indeterminate()` reports the current mode. Pass a Boolean to `indeterminate(_:)` or `setIndeterminate(_:)` to switch modes:

```swift
if !progressBar.indeterminate() {
    progressBar.setIndeterminate(true)
}
```
