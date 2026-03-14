# ExpandableListView

`ExpandableListView` is Droid's classic two-level list: groups can expand to reveal child rows. The current wrapper exposes group expansion/collapse, packed-position conversions, adapter access, selection, indicators, and JNI-compatible group/child listener bridges.

## Use it when…

Use `ExpandableListView` when your existing Android UI is naturally modeled as expandable groups and child rows.

For new highly customized sectioned lists, `RecyclerView` may be easier to evolve.

## How it behaves

The current wrapper's group, adapter, indicator, listener, and selection methods operate on the live Android instance.

It does not currently expose queued builder properties for this surface.

## Examples

### Expand or collapse a live group

```swift
let list = ExpandableListView()

list.expandGroup(0)
list.collapseGroup(0)
```

### Inspect packed positions

```swift
let packed = list.packedPositionForChild(2, 4)
let group = list.packedPositionGroup(packed)
let child = list.packedPositionChild(packed)
```

These methods operate on Android's live list state.

## Initializers

```swift
ExpandableListView()
ExpandableListView(id: 1001)
```

## Groups

`expandGroup(_:)` and `expandGroup(_:_:)` expand a group, optionally controlling animation.

`collapseGroup(_:)` collapses it.

`isGroupExpanded(_:)` reads current state.

## Adapter

`setAdapter(_:)` accepts a JNI-compatible Android `ListAdapter` object.

`adapter()` and `expandableListAdapter()` return the live adapter objects exposed by Android.

## Packed positions

The wrapper exposes Android's packed-position conversion helpers:

- `expandableListPosition(_:)`
- `flatListPosition(_:)`
- `packedPositionForChild(_:_:)`
- `packedPositionForGroup(_:)`
- `packedPositionChild(_:)`
- `packedPositionGroup(_:)`
- `packedPositionType(_:)`

## Selection

`setSelectedChild(_:_:_:)` selects a child and can expand its group.

`setSelectedGroup(_:)` selects a group.

`selectedId()` and `selectedPosition()` read the live selection.

## Indicators and listener bridges

The wrapper exposes child/group divider and indicator setters plus absolute/relative indicator bounds.

Child-click, group-click, group-collapse, group-expand, and item-click setters accept JNI-compatible Android listener objects.

Droid exposes the listener bridges but does not construct those listener objects for you.

## Lower-level helpers

State restoration, accessibility, RTL, drawing, and `performItemClick` helpers are also available.
