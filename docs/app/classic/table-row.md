# TableRow

`TableRow` is the row container used inside [→ TableLayout](table-layout.md).

Think of it as a horizontal strip of cells. Each row usually contains the same number of children so columns line up.

Android reference: [android.widget.TableRow](https://developer.android.com/reference/android/widget/TableRow)

## Use it when…

Use it when you’re building a small table-like layout with aligned columns.

If you need complex row content, you’ll typically get better results with [→ LinearLayout](linear-layout.md) (and accept that columns won’t align automatically).

## How it behaves

Children are laid out left-to-right. Each child is a **cell** and carries `TableRow` layout params:

- `column(_:)` places the cell at a zero-based column index.
- `span(_:)` makes the cell occupy several adjacent columns.

Those two modifiers belong to cells **inside** a `TableRow`. The `TableRow` itself, when placed in a `TableLayout`, uses `TableLayout` layout params instead.

## Examples

### Example 1: A simple two-column row

```swift
TableRow {
    TextView("Subtotal")
    TextView("$12.34")
}
```

The two cells occupy the first two columns of the row.

### Example 2: A cell that spans both columns

```swift
TableRow {
    TextView("Shipping")
        .span(2)
        .textSize(18)
}
```

One cell occupies two columns instead of two separate cells.

### Example 3: Pin a cell to a column

```swift
TableRow {
    TextView("Total")
        .column(0)
    TextView("$24.68")
        .column(1)
        .textSize(18)
}
```

The first cell sits in column 0 and the second in column 1.

## Initializers

```swift
TableRow()
TableRow(id: 1001)
TableRow {
    TextView("Left")
    TextView("Right")
}
```

## Layout params

Apply these to a **cell** inside a `TableRow`:

| Modifier | Meaning |
|---|---|
| `column(_:)` | zero-based column index for this cell |
| `span(_:)` | number of adjacent columns this cell occupies |

A spanning header row is separate from numbered cells in later rows:

```swift
TableLayout {
    TableRow {
        TextView("Billing")
            .span(2)
            .textSize(18)
    }
    TableRow {
        TextView("Next charge")
            .column(0)
        TextView("$9.99")
            .column(1)
    }
}
```

`column` and `span` are `TableRow` cell fields. Do not use them as row-level params on the `TableRow` itself; a `TableRow` child of `TableLayout` uses `TableLayout` layout params.
