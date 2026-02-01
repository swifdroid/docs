# AdapterView

`AdapterView` is the base type behind classic Android widgets whose visible items come from an adapter.

You normally encounter it through concrete controls such as `ListView`, `GridView`, or `Spinner` rather than creating an `AdapterView` directly.

## Use it when…

Use `AdapterView` as a type boundary when code needs to work with a classic adapter-backed view without depending on one concrete widget.

For new list and grid interfaces, prefer `RecyclerView` when its more flexible recycling model fits the screen.

## How it behaves

Droid currently provides the generic `AdapterView<T: Adapter>` base and its inherited view-group behavior. Adapter population, selection, and callback APIs are not exposed on this type yet.

That makes the type useful mainly as a base class and generic constraint in current code.

## Example

A helper can accept any concrete `AdapterView` whose adapter type satisfies `Adapter`:

```swift
func fillAvailableWidth<A: Adapter>(_ view: AdapterView<A>) {
    view.width(.matchParent)
}
```

The helper works with the common view surface without assuming list-, grid-, or spinner-specific behavior.

## Notes

Do not instantiate the generic base just to display content. Choose the concrete adapter-backed widget that matches the UI you are building.
