# TableLayout

`TableLayout` arranges children in rows (usually [→ TableRow](table-row.md))—it’s the old-school way to build “tables” in classic Android UIs.

For modern, flexible layouts, you’ll often reach for [→ LinearLayout](linear-layout.md) or [→ RecyclerView](../x/recycler-view.md) instead, but `TableLayout` can still be handy for small, fixed sets of key/value info.

Android reference: [android.widget.TableLayout](https://developer.android.com/reference/android/widget/TableLayout)

## Use it when…

Use it when your UI is truly tabular: rows of values, simple summaries, or compact settings where columns line up.

## How it behaves

`TableLayout` lays out each `TableRow` and tries to keep columns aligned. It’s not meant for large datasets—think “a dozen rows”, not “a scrollable feed”.

Ownership of layout params is split:

- a `TableRow` child of `TableLayout` uses `TableLayout` layout params;
- each cell inside that row uses `TableRow` layout params (`column(_:)`, `span(_:)`).

## Examples

### Example 1: Key/value summary

```swift
TableLayout {
    TableRow {
        TextView("Name")
        TextView("Mihael")
    }
    TableRow {
        TextView("Plan")
        TextView("Pro")
    }
}
.padding(16)
```

Two aligned columns of label/value pairs.

### Example 2: A cell that spans both columns

```swift
TableLayout {
    TableRow {
        TextView("Billing")
            .span(2)
            .textSize(18)
    }
    TableRow {
        TextView("Next charge")
        TextView("$9.99")
    }
}
.padding(16)
```

The first row has a single cell spanning two columns; the second row has two ordinary cells.

## Initializers

```swift
TableLayout()
TableLayout(id: 1001)
TableLayout {
    TableRow {
        TextView("Name")
    }
}
```

## Layout params

`TableLayout` owns row-level params for its `TableRow` children.

Cell-level placement (`column(_:)`) and multi-column occupancy (`span(_:)`) belong to cells inside each row — see [→ TableRow](table-row.md).
