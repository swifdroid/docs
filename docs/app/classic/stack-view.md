# StackView

`StackView` is a legacy Android widget for presenting a stack-like collection of views.

Droid currently exposes construction and inherited view behavior, not the adapter and animation controls needed for a full interactive stack.

## Use it when…

Use `StackView` when maintaining an existing screen that already relies on the Android widget.

For new collection UI, prefer a modern component such as `RecyclerView` or an explicit custom transition.

## How it behaves

Use `StackView` as a view that can be created, identified, sized, and styled through inherited view APIs.

Adapter population and stack-navigation helpers are not exposed on this type.

## Example

```swift
StackView()
    .width(.matchParent)
    .height(220)
```

## Initializers

```swift
StackView()
StackView(id: 1001)
```

## Notes

Keep application logic independent of adapter or animation controls that are not exposed by this type yet.
