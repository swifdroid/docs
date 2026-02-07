# Toast

`Toast` shows a short-lived message that does not require a response.

Use it for lightweight feedback such as "Saved" or "Copied". When the user may need an action such as Undo, use an interactive surface such as `Snackbar` instead.

## Use it when…

Use a toast for brief, low-risk information that can disappear automatically.

Do not use it for critical errors or decisions the user must acknowledge.

## How it behaves

`Toast.makeText(_:context:duration:)` creates the message. `show()` displays it, and `cancel()` can hide it early.

The duration defaults to `.short`; use `.long` when the message needs a little more reading time.

## Examples

### Show a short confirmation

```swift
Toast.makeText("Saved", context: self)?.show()
```

### Use the longer duration

```swift
Toast.makeText(
    "Upload finished",
    context: self,
    duration: .long
)?.show()
```

### Update a toast before showing it

```swift
Toast.makeText("Working…", context: self)?
    .text("Done")
    .duration(.long)
    .show()
```

## Methods

### `makeText(_:context:duration:)`

Creates a text toast and returns it when Android can create the underlying object.

### `show()`

Displays the toast and returns the same Toast instance for chaining.

### `duration(_:)`

Changes the duration between `.short` and `.long`.

### `text(_:)`

Updates the toast text.

### `cancel()`

Hides a toast that is currently showing.
