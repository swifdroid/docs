# RadioGroup

`RadioGroup` keeps a small set of radio buttons mutually exclusive: selecting one option clears the previous selection.

It is useful for choices such as billing period, delivery speed, or a small list of modes where exactly one option should be active at a time.

## Use it when…

Use `RadioGroup` when the user needs to choose one item from a short, visible set of alternatives.

For independent on/off choices, use separate checkable controls instead. For a long or dynamic list, a recycled list with explicit selection state is usually a better fit.

## How it behaves

Place `RadioButton` children inside the group and give each button a stable id when application code needs to select or identify it.

`check(_:)`, `clearCheck()`, and `checkedRadioButtonId()` are live operations. Call them after the Android view has been realized.

The checked-change listener surface accepts an Android/JNI-compatible listener object. There is no Swift closure convenience for group selection changes in the current API.

## Examples

### Build a billing-period choice

```swift
RadioGroup(id: 2000) {
    RadioButton(id: 2001)
        .text("Monthly")

    RadioButton(id: 2002)
        .text("Annual")

    RadioButton(id: 2003)
        .text("Lifetime")
}
.orientation(.vertical)
```

The group coordinates the three buttons so only one can remain selected.

### Select an option from application state

After the group is on screen, select a button by its id:

```swift
func chooseAnnualPlan(_ group: RadioGroup) {
    group.check(2002)
}
```

The button with id `2002` becomes the selected radio button.

### Read or clear the live selection

```swift
let selectedID = group.checkedRadioButtonId()

group.clearCheck()
```

`checkedRadioButtonId()` returns the current selected button id. `clearCheck()` removes the group's current selection.

## Initializers

Create an empty group, provide its radio buttons directly, or assign an id for later access:

```swift
RadioGroup()

RadioGroup {
    RadioButton(id: 2001)
        .text("Standard")
    RadioButton(id: 2002)
        .text("Express")
}

RadioGroup(id: 2000)
```

Use the inherited `LinearLayout` configuration when the options should be vertical or horizontal.

## Methods

### Select a radio button

`check(_:)` selects the child radio button whose id matches the supplied value.

```swift
group.check(2002)
```

This is a live call, so use it after the native group exists.

### Clear the selection

`clearCheck()` removes the current checked radio button.

```swift
group.clearCheck()
```

Use it when the UI is allowed to return to a no-selection state.

### Read the selected id

`checkedRadioButtonId()` reads the id of the currently checked child from the live group.

```swift
let selectedID = group.checkedRadioButtonId()
```

The Android API uses `-1` when no radio button is selected.

### Attach Android listener objects

`onCheckedChangeListener(_:)` configures an Android-compatible `RadioGroup.OnCheckedChangeListener` object.

```swift
group.onCheckedChangeListener(listener)
```

The listener can be configured before realization, but `listener` must already be a JNI-compatible listener object; this method does not accept a Swift closure.

`onHierarchyChangeListener(_:)` follows the same pattern for Android hierarchy-change listener objects.
