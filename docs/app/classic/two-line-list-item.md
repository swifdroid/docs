# TwoLineListItem

`TwoLineListItem` wraps Android's legacy two-line list row. It inherits `RelativeLayout` and exposes the two internal `TextView` handles Android names `text1` and `text2`.

## Use it when…

Use it when you are integrating with an existing Android layout or adapter that specifically uses `TwoLineListItem`.

For new UI, a small custom row built from normal Droid views is usually clearer.

## How it behaves

The wrapper has no queued row-specific properties.

`text1()` and `text2()` read the live Android child views.

## Example

```swift
let row = TwoLineListItem()

if let title = row.text1() {
    // Use the live TextView handle.
}

if let subtitle = row.text2() {
    // Use the second live TextView handle.
}
```

The accessors require Android's row instance to exist.

## Initializers

```swift
TwoLineListItem()
TwoLineListItem(id: 1001)
```

## Text handles

`text1()` returns the live `TextView?` identified by Android as the first line.

`text2()` returns the live `TextView?` identified as the second line.

The wrapper does not expose dedicated queued setters for either line; configure the returned live `TextView` objects or use your own row layout when you need fully declarative text composition.

## Lower-level helper

`onFinishInflate()` forwards Android's inflation-finalization callback for low-level integrations.
