# FragmentContainerView

`FragmentContainerView` wraps AndroidX `FragmentContainerView`.

It inherits Droid's `FrameLayout` wrapper and is intended to provide the AndroidX container used for fragment transactions.

## Use it when…

Use it when a FragmentActivity needs a dedicated view ID where fragments will be added or replaced.

## Add the container to your screen

```swift
body {
    FragmentContainerView(id: 300)
        .width(.matchParent)
        .height(.matchParent)
}
```

Give the container an explicit ID when a FragmentTransaction needs to target it.

## Replace the fragment in the container

```swift
func show(
    _ fragment: Fragment,
    in activity: FragmentActivity
) {
    guard
        let manager = activity.supportFragmentManager(),
        let transaction = manager.beginTransaction(),
        let replacement = transaction.replace(
            containerViewId: 300,
            fragment: fragment
        )
    else { return }

    _ = replacement.commit()
}
```

The transaction API is owned by `FragmentManager` and `FragmentTransaction`; `FragmentContainerView` itself currently exposes container construction and inherited `FrameLayout` behavior.
