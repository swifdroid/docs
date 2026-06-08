# FragmentContainerView

`FragmentContainerView` is a fragment-specific container based on `FrameLayout`. A fragment transaction can address this view by its ID.

Pass an explicit ID when transaction code needs a known container ID. If `id` is omitted, the inherited `View` initializer generates one.

The component provides an initializer without a body closure and a builder-closure overload. Fragment manager and transaction APIs belong to `FragmentActivity`, `FragmentManager`, and `FragmentTransaction`.

## Use it when…

Use it when a layout needs a dedicated fragment host that a transaction can target by ID. An explicit ID lets the layout and the transaction refer to the same slot.

## How it behaves

`FragmentContainerView` extends `FrameLayout`, so its inherited `width` and `height` modifiers can request `.matchParent` sizing for a child of a `FrameLayout`. These modifiers express a sizing request; they do not establish final geometry or window attachment.

`FragmentActivity` provides access to a fragment manager. The manager creates transactions and reports whether its state has been saved; a transaction can request replacement for a container ID. These APIs are separate from the container view.

## Examples

### Add a fragment slot to a host

This builds a `FrameLayout` with a `FragmentContainerView` child whose ID is `300`. The child requests the host's available width and height.

```swift
@MainActor
func fragmentHost() -> FrameLayout {
    FrameLayout {
        FragmentContainerView(id: 300)
            .width(.matchParent)
            .height(.matchParent)
    }
}
```

### Request a fragment replacement

When the guard passes, this requests a replacement transaction for the container with ID `300`. The helper skips transaction setup when the manager reports saved state and checks each optional API result.

```swift
@MainActor
func install(_ fragment: Fragment, in activity: FragmentActivity) {
    guard let manager = activity.supportFragmentManager(),
          !manager.isStateSaved(),
          let transaction = manager.beginTransaction(),
          let replacement = transaction.replace(
              containerViewId: 300,
              fragment: fragment
          ),
          replacement.commit() != nil else {
        return
    }
}
```

`commit()` schedules the request on the main thread; it does not execute immediately. The source warns that committing after the containing Activity has saved its state throws. The saved-state check is a guard, not a guarantee that the commit succeeds. A non-nil commit ID does not prove execution or display.

## Initializers

- `init(id: Int32? = nil)` creates the container without a content closure.
- `init(id: Int32? = nil, @BodyBuilder content: BodyBuilder.SingleView)` accepts a builder closure.

Both initializers accept an optional ID. With no explicit ID, the inherited `View` initializer generates one. Supply an explicit ID when another API needs to target this container by a known value.

## Notes

The builder initializer exists, but its declaration alone does not establish that arbitrary ordinary views are compatible as children of a fragment-managed container or that the closure renders fragments. Keep ordinary layout views outside `FragmentContainerView`.
