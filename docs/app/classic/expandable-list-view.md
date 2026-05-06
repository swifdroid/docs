# ExpandableListView

`ExpandableListView` is Android’s two-level list: groups that expand to show their children.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; expandable adapters, group/child APIs, and expansion listeners are not wrapped yet.

## Use it when…

Use `ExpandableListView` when the screen needs classic expandable group/child list behavior and you can wait for the adapter surface to land.

For a flat list name only, see [`ListView`](list-view.md). For highly customized lists, prefer `RecyclerView`.

## How it behaves

`ExpandableListView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose adapter assignment, expand/collapse helpers, or group/child selection APIs at this pin.

## Examples

### Place an expandable list

```swift
ExpandableListView(id: 2101)
    .padding(h: 0, v: 0)
```

## Initializers

```swift
ExpandableListView()
ExpandableListView(id: 2101)
```

## Notes

Android’s `ExpandableListView` is defined by `ExpandableListAdapter` and group/child callbacks. None of that is part of the current Droid surface.
