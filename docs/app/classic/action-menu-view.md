# ActionMenuView

`ActionMenuView` is Android’s action-bar menu container.

In the current Droid source pin it inherits [`LinearLayout`](linear-layout.md) and has a single-child content initializer plus its own layout-param class. Menu presentation APIs are not wrapped yet.

## Use it when…

Use `ActionMenuView` when the screen needs the classic Android action-menu container name and menu APIs can land later.

For ordinary vertical/horizontal stacks, use [`LinearLayout`](linear-layout.md) directly.

## How it behaves

`ActionMenuView` can take declarative content and keeps the current `LinearLayout` surface, including `orientation(_:)` and shared view configuration such as padding and `onClick(_:)`.

The current type does **not** expose `setPresenter(_:)`, overflow helpers, or menu-model binding at this pin.

## Examples

### Place an action menu row

```swift
ActionMenuView(id: 4101) {
    TextView("Action")
}
```

## Initializers

```swift
ActionMenuView()
ActionMenuView(id: 4101)

ActionMenuView(id: 4101) {
    // single child content via BodyBuilder
}
```

## Layout params

`ActionMenuView` declares its own `LayoutParams` class at this pin. Use the shared layout-param surface when configuring children.

## Notes

Android’s `ActionMenuView` binds an action menu model and presenter. That surface is not part of the current Droid API.
