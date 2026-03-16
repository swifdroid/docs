# CheckedTextView

`CheckedTextView` is Droid's `TextView` wrapper with Android's checkable state and check-mark drawable. The current wrapper exposes queued check-mark styling plus live checked-state actions and getters.

## Use it when…

Use `CheckedTextView` for classic rows where text and selection state are represented by one Android view.

For a form-style boolean control, `CheckBox` is usually more explicit.

## How it behaves

Check-mark drawable/tint configuration and `setVisibility(_:)` are queued Droid view properties.

`checked(_:)`, `toggle()`, `isChecked()`, and the lower-level drawing/state helpers operate on the live Android instance.

Because `CheckedTextView` inherits `TextView`, normal text configuration remains available.

## Examples

### Configure the check mark before mounting

```swift
CheckedTextView()
    .checkMarkDrawable(1001)
```

### Change checked state on a live view

```swift
let item = CheckedTextView()

item.checked(true)
item.toggle()

let selected = item.isChecked()
```

The checked-state methods are live actions; `checked(_:)` is not a queued builder property in the current wrapper.

## Initializers

```swift
CheckedTextView()
CheckedTextView(id: 1001)
```

## Checked state

`checked(_:)` immediately calls Android's `setChecked`.

`toggle()` flips the live checked state.

`isChecked()` reads it.

## Check-mark drawable

`checkMarkDrawable(_: Drawable?)` queues a drawable.

`checkMarkDrawable(_: Int)` queues a drawable resource ID.

`checkMarkDrawable()` reads the live drawable.

## Tinting

Queued setters include:

- `checkMarkTintList(_:)`
- `checkMarkTintBlendMode(_:)`
- `checkMarkTintMode(_:)`

Matching getters expose the live tint values where available.

## Lower-level helpers

The wrapper also exposes drawable-state, hotspot, padding, RTL, accessibility, save/restore-state, drawing, and drawable-verification helpers.
