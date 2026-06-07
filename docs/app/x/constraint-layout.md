# ConstraintLayout

Use `ConstraintLayout` to describe where a child view belongs by relating its edges to the parent or to another view. This keeps those relationships on the views they position.

In Droid, the constraint methods are layout parameters on each child `View`. Use them to align edges or place one child beside another by its ID.

## Use it when…

Use it when a screen is easier to describe with explicit parent-edge and sibling relationships than with nested layout groups.

## How it behaves

Each constraint is configured on the child view whose position it describes. `ConstraintLayout` accepts its supported constraint parameters along with the inherited size and margin parameters.

Directional anchors such as `startToParent()` follow the layout direction: start is left in LTR and right in RTL. An ID-based anchor such as `topToBottom(id)` relates the child's top edge to the referenced view's bottom edge.

## Examples

This aligns the start and top edges of `Account status` with the parent:

```swift
@MainActor
func accountPanel() -> FrameLayout {
    FrameLayout {
        ConstraintLayout {
            TextView("Account status")
                .startToParent()
                .topToParent()
        }
    }
}
```

This places `Plan: Standard` below the child with ID `1` and aligns its start edge with the parent:

```swift
@MainActor
func accountDetails() -> FrameLayout {
    FrameLayout {
        ConstraintLayout {
            TextView(id: 1, "Account")
                .startToParent()
                .topToParent()

            TextView("Plan: Standard")
                .startToParent()
                .topToBottom(1)
        }
    }
}
```

## Initializers

`ConstraintLayout` has an initializer with an optional `id` and a builder initializer that accepts child content and an optional `id`. View construction and body-builder closures use main-actor-isolated APIs, so keep these calls in a `@MainActor` context as shown above.

Use the ID-only form when the container needs an ID without a child-content builder. This constructs a `ConstraintLayout` whose view ID is `2`:

```swift
@MainActor
func identifiedLayout() -> ConstraintLayout {
    ConstraintLayout(id: 2)
}
```

## Layout params

Set constraints on the child view:

- Use parent anchors such as `startToParent()` and `topToParent()` to align a child with a parent edge.
- Use ID-based anchors such as `startToStart(id)`, `startToEnd(id)`, `topToBottom(id)`, and `baselineToBaseline(id)` to relate one child to another.
- Constraint-specific parameters also cover bias, chains, circular positioning, aspect ratio, and margins for views whose visibility is `gone`.

The container applies its supported constraint parameters together with the standard size and margin parameters inherited from `View`.
