# ListView

`ListView` is Droid's classic vertical adapter-backed list. The current wrapper exposes headers and footers, divider configuration, focus behavior, selection, smooth scrolling, adapter access, and lower-level list lifecycle helpers.

For new highly customized lists, `RecyclerView` is usually more flexible, but `ListView` remains useful for classic Android list behavior.

## Use it when…

Use `ListView` when you need header/footer rows, classic Android selection behavior, or compatibility with a `ListAdapter`-based screen.

## How it behaves

Divider, divider-height, header/footer divider flags, item-focus behavior, overscroll drawables, cache-color hint, and `selection(_:)` are queued Droid view properties.

Adapter assignment, header/footer mutation, smooth scrolling, and most getters operate on the live Android instance.

## Examples

### Configure list presentation before mounting

```swift
ListView()
    .dividerHeight(1)
    .headerDividersEnabled(true)
    .footerDividersEnabled(true)
    .itemsCanFocus(false)
    .selection(0)
```

### Scroll a mounted list

```swift
let list = ListView()

list.smoothScrollToPosition(20)
list.smoothScrollByOffset(3)
```

The scrolling methods require a live Android instance.

## Initializers

```swift
ListView()
ListView(id: 1001)
```

The wrapper also has low-level initializers for an existing Android `JObject` plus an `ActivityContext` link.

## Adapter

`setAdapter(_:)` assigns a JNI-compatible Android `ListAdapter` object to the live list.

`adapter()` returns the current adapter object.

Adapter assignment is not queued in the current wrapper.

## Headers and footers

`addHeaderView(_:)` and `addFooterView(_:)` add fixed live rows. Overloads accept an optional data object and an `isSelectable` flag.

`removeHeaderView(_:)` and `removeFooterView(_:)` remove them.

`headerViewsCount()` and `footerViewsCount()` read the current counts.

## Dividers and overscroll

Queued setters:

- `divider(_:)`
- `dividerHeight(_:)`
- `headerDividersEnabled(_:)`
- `footerDividersEnabled(_:)`
- `overscrollHeader(_:)`
- `overscrollFooter(_:)`

Matching getters read the live Android values.

## Selection and scrolling

`selection(_:)` is queued.

`selectionAfterHeaderView()`, `smoothScrollToPosition(_:)`, and `smoothScrollByOffset(_:)` are live actions.

## Lower-level helpers

The wrapper also exposes checked-item IDs, keyboard/accessibility hooks, child-layout helpers, remote-views adapter support, and Android's header-list adapter wrapping helpers.
