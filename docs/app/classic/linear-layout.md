# LinearLayout

`LinearLayout` arranges children along one axis: vertically as a column or horizontally as a row.

It is a practical choice for forms, settings sections, button rows, and other layouts whose structure is naturally one-dimensional.

## Use it when…

Use `LinearLayout` when the content can be described as a row or a column and children do not need to overlap.

For overlay content, use [`FrameLayout`](frame-layout.md). For more complex relationships between views, use a layout that supports explicit constraints or relative rules.

## How it behaves

`orientation(_:)` selects vertical or horizontal flow. In a vertical layout children are placed from top to bottom; in a horizontal layout they are placed from start to end.

Child `weight(_:)` values distribute remaining space between weighted children. Container methods such as `gravityForSubviews(_:)` and `showDividers(_:)` configure how the layout presents its children.

## Examples

### Build a vertical account section

```swift
LinearLayout {
    TextView("Account")
        .textSize(18)

    TextView("Signed in as mihael@example.com")
        .textSize(12)
        .textColor(.lightGray)
}
.orientation(.vertical)
.padding(16)
```

The labels appear one below the other in declaration order.

### Give one item the remaining width

```swift
LinearLayout {
    TextView("Storage")
        .weight(1)

    TextView("2.1 GB")
}
.orientation(.horizontal)
.padding(16)
```

The Storage label receives the remaining horizontal space, leaving the value at the end of the row.

### Create two equal-width actions

```swift
LinearLayout {
    Button("Cancel")
        .width(0)
        .weight(1)

    Button("Save")
        .width(0)
        .weight(1)
}
.orientation(.horizontal)
.padding(16)
```

Both buttons request the same weight, so they share the available row width evenly.

## Initializers

Create an empty layout, provide children directly, or assign an id when a stable reference is useful:

```swift
LinearLayout()

LinearLayout {
    TextView("Child")
}

LinearLayout(id: 1001)
```

## Layout params

Layout params such as `weight(_:)` and `gravityInParent(_:)` are applied to children inside the `LinearLayout`.

```swift
LinearLayout {
    TextView("Left")
        .weight(1)

    TextView("Right")
}
.orientation(.horizontal)
```

## Methods

### Choose the orientation

`orientation(_:)` switches between vertical and horizontal child flow.

```swift
LinearLayout {
    TextView("First")
    TextView("Second")
}
.orientation(.vertical)
```

### Align the children

`gravityForSubviews(_:)` controls how children are aligned within unused space in the layout.

```swift
LinearLayout {
    TextView("Centered")
}
.orientation(.vertical)
.gravityForSubviews(.centerHorizontal)
```

### Control divider positions

`showDividers(_:)` selects where the layout shows its configured child dividers.

```swift
LinearLayout {
    TextView("Profile")
    TextView("Notifications")
    TextView("About")
}
.orientation(.vertical)
.showDividers(.middle)
```
