# Button

`Button` presents a text action that the user can tap. Use it for explicit choices such as Save, Continue, Retry, or Delete when the action should be visible as a control rather than attached to another piece of content.

A button uses the same text surface as [`TextView`](text-view.md), so text size and other verified text configuration can be applied directly. Click handling comes from the shared view interaction API through `onClick(_:)`.

## Use it when…

Use a `Button` when the user should deliberately trigger an action and the action deserves its own visible control. Short labels that describe the result, such as "Save" or "Try again", make the intent clear.

For text that only communicates information and is not interactive, use `TextView` instead.

## How it behaves

The button participates in layout like other views and displays its label through the inherited text behavior. Register `onClick(_:)` to run work when the user activates it.

The click handler is attached as part of view configuration, so it can be declared alongside the button in the body builder. Text styling remains available through the inherited `TextView` behavior.

## Examples

### Run an action when tapped

```swift
@State var message = "Nothing saved yet"

body {
    VStack {
        TextView($message)

        Button("Save")
            .onClick {
                message = "Saved"
            }
    }
    .padding(16)
}
```

Tapping Save changes the state, and the status label updates to show the result.

### Style the label while keeping button behavior

```swift
body {
    Button("Continue")
        .textSize(18)
        .width(.matchParent)
        .onClick {
            Log.i("Continue tapped")
        }
}
```

Text configuration is inherited from `TextView`; the button still handles the click through `onClick(_:)`.

## Initializers

A button can be created with a text value and may also use an explicit view id:

```swift
Button("Save")
Button(id: 1001, "Continue")
```

## Methods

### Click handling

`onClick(_:)` registers the action that runs when the button is clicked.

```swift
Button("Retry")
    .onClick {
        Log.i("Retry requested")
    }
```

The shared view API also has an event form when the click event itself is needed.

### Text configuration

Because `Button` inherits the text behavior documented on [`TextView`](text-view.md), you can configure its label with methods such as `text(_:)`, `textSize(_:_:)`, `maxLines(_:)`, and `gravity(_:)` when those choices are useful for the control.

```swift
Button("Open details")
    .textSize(16)
    .maxLines(1)
```
