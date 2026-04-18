# GridLayout

`GridLayout` places children in a fixed grid of rows and columns. It fits small, known structures: keypads, settings groups, two-column fact rows, and dashboards where the cell count is decided at layout time.

Each child gets a `rowSpec` and `columnSpec` that say where it starts, how wide/tall it is, and how it aligns inside the cell. The parent can also set overall row/column counts and whether missing cells keep Android’s default margins.

For large or dynamic collections, prefer a recycled list instead. `GridLayout` is for compact, intentional matrices.

## Use it when…

Use `GridLayout` when the screen shows a small, fixed matrix of controls or labels and you want each cell positioned explicitly.

Do not use it for infinite grids, adapter-driven feeds, or masonry layouts. Those need a recycled or custom layout strategy.

## How it behaves

Children occupy cells described by `rowSpec(...)` / `columnSpec(...)`. Overloads accept `start`, optional `size`, optional `weight`, and optional `alignment`.

The grid itself exposes live configuration: `rowCount`, `columnCount`, `orientation`, `alignmentMode`, `useDefaultMargins`, and row/column order preservation. Matching `set*` methods are immediate void calls on the native grid.

`GridLayout.Alignment` controls how a child sits inside its cell. The exposed cases are `leading`, `trailing`, `top`, `bottom`, `start`, `end`, `left`, `right`, `center`, `baseline`, and `fill`.

## Examples

### Build a 2×2 action grid

```swift
GridLayout {
    Button("A")
        .rowSpec(start: 0)
        .columnSpec(start: 0)
    Button("B")
        .rowSpec(start: 0)
        .columnSpec(start: 1)
    Button("C")
        .rowSpec(start: 1)
        .columnSpec(start: 0)
    Button("D")
        .rowSpec(start: 1)
        .columnSpec(start: 1)
}
.padding(16)
```

Four equal buttons appear in a 2×2 matrix.

### Lay out a two-column info block

```swift
GridLayout {
    TextView("Name")
        .rowSpec(start: 0)
        .columnSpec(start: 0)
    TextView("Mihael")
        .rowSpec(start: 0, alignment: .leading)
        .columnSpec(start: 1)

    TextView("Version")
        .rowSpec(start: 1)
        .columnSpec(start: 0)
    TextView("1.0")
        .rowSpec(start: 1, alignment: .leading)
        .columnSpec(start: 1)
}
.padding(16)
```

Labels and values line up in two columns.

### Size a grid and span a cell

```swift
let grid = GridLayout {
    TextView("Header")
        .rowSpec(start: 0, size: 1)
        .columnSpec(start: 0, size: 2)
    Button("Left")
        .rowSpec(start: 1)
        .columnSpec(start: 0)
    Button("Right")
        .rowSpec(start: 1)
        .columnSpec(start: 1)
}

grid.setRowCount(2)
grid.setColumnCount(2)
grid.setUseDefaultMargins(true)
```

The header spans both columns. `setRowCount` / `setColumnCount` / `setUseDefaultMargins` are void calls that configure the live grid.

## Initializers

```swift
GridLayout()

GridLayout(id: 1001)

GridLayout(id: 1001) {
    // children
}
```

Pass an id when other code must find the grid later. The content block is the ordinary Droid view builder.

## Layout params

Place children with `rowSpec` and `columnSpec`. Overloads combine `start`, `size`, `weight`, and `alignment`:

```swift
TextView("A")
    .rowSpec(start: 0, size: 1, alignment: .fill, weight: 1)
    .columnSpec(start: 0, size: 1, alignment: .center, weight: 1)

TextView("B")
    .rowSpec(start: 1, weight: 2)
    .columnSpec(start: 1, alignment: .end)
```

`start` is zero-based. `size` is the span. `weight` distributes extra space. `alignment` is a `GridLayout.Alignment` case.

## Methods

### Grid counts and orientation

`rowCount()` / `setRowCount(_:)` and `columnCount()` / `setColumnCount(_:)` read or set the grid size. `orientation()` / `setOrientation(_:)` read or set layout orientation.

```swift
grid.setRowCount(3)
grid.setColumnCount(2)
grid.setOrientation(orientation)
```

The setters are immediate void calls.

### Alignment mode and default margins

`alignmentMode()` / `setAlignmentMode(_:)` control cell alignment rules. `useDefaultMargins()` / `setUseDefaultMargins(_:)` toggle Android’s default cell margins.

```swift
grid.setAlignmentMode(mode)
grid.setUseDefaultMargins(true)
```

### Order preservation

`isRowOrderPreserved()` / `setRowOrderPreserved(_:)` and `isColumnOrderPreserved()` / `setColumnOrderPreserved(_:)` decide whether undefined cells keep declared order.

```swift
grid.setRowOrderPreserved(true)
grid.setColumnOrderPreserved(true)
```

## Notes

`rowSpec` / `columnSpec` are layout params on the child, not methods on the grid. Configure the grid with its `set*` methods before measurement when counts or margins must be exact.
