# ListView

`ListView` is Android’s classic vertical adapter-backed list.

In the current Droid source pin it is a placeable view control. Shared view interaction and layout APIs work; adapter assignment, headers/footers, dividers, selection, and scroll helpers are not wrapped yet.

For new highly customized lists, `RecyclerView` is usually more flexible. `ListView` remains the name for classic Android list behavior.

## Use it when…

Use `ListView` when you need the classic Android list widget name in the hierarchy and will fill it later as the wrapper grows.

## How it behaves

`ListView` participates in the view hierarchy like other controls. You can assign an id, attach `onClick(_:)`, set padding, and control `enabled(_:)` / `visibility(_:)`.

The current type does **not** expose `adapter(_:)`, header/footer APIs, divider configuration, `selection(_:)`, smooth scrolling, or list listeners at this pin.

## Examples

### Place a list container

```swift
ListView(id: 1301)
    .padding(h: 0, v: 0)
```

### Show an empty list area while data loads

```swift
ListView(id: 1302)
    .enabled(false)
```

## Initializers

```swift
ListView()
ListView(id: 1301)
```

## Notes

Android’s `ListView` is defined by its adapter and row behavior. None of that is part of the current Droid surface. Treat this page as placement plus shared view behavior only.
