# GridView

`GridView` is Droid's classic adapter-backed scrolling grid. The current wrapper exposes column count and width, horizontal/vertical spacing, gravity, stretch mode, adapter access, selection, scrolling, and lower-level layout/accessibility helpers.

## Use it when…

Use `GridView` when you need Android's classic `ListAdapter` grid behavior.

For richer modern layouts, a grid-configured `RecyclerView` is usually more flexible.

## How it behaves

The current `GridView` wrapper exposes its grid configuration as immediate live-instance setters.

Unlike many Droid view wrappers, `setNumColumns(_:)`, `setColumnWidth(_:)`, spacing setters, gravity, stretch mode, adapter assignment, and selection are not queued builder properties.

## Example

```swift
let grid = GridView()

grid.setNumColumns(3)
grid.setColumnWidth(120)
grid.setHorizontalSpacing(8)
grid.setVerticalSpacing(8)
```

These setters require a live Android instance to have an effect.

## Initializers

```swift
GridView()
GridView(id: 1001)
```

## Grid geometry

Live setters:

- `setNumColumns(_:)`
- `setColumnWidth(_:)`
- `setHorizontalSpacing(_:)`
- `setVerticalSpacing(_:)`
- `setGravity(_:)`
- `setStretchMode(_:)`

Live getters expose the current and requested column width/spacing, number of columns, gravity, stretch mode, and vertical spacing.

## Adapter and selection

`setAdapter(_:)` accepts a JNI-compatible Android `ListAdapter` object.

`adapter()` returns the live adapter object.

`setSelection(_:)` changes the current selection immediately.

The current wrapper does not provide queued adapter or selection properties.

## Scrolling

`smoothScrollToPosition(_:)` scrolls to an adapter position.

`smoothScrollByOffset(_:)` scrolls by an item offset.

The wrapper also exposes Android's vertical scroll extent, offset, and range values.

## Lower-level helpers

Layout-animation, measurement, keyboard, accessibility, remote-views adapter, and child-layout hooks are available for lower-level integrations.
