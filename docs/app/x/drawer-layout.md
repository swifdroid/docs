# DrawerLayout

`DrawerLayout` wraps AndroidX `DrawerLayout` as a Droid `ViewGroup`.

Use it when your screen needs the AndroidX drawer container while keeping the rest of the hierarchy in Droid's builder style.

## Use it when…

Use `DrawerLayout` as the root container for a screen that will host drawer content.

The current wrapper focuses on container construction and inherited `ViewGroup` behavior.

## Build the container

```swift
body {
    DrawerLayout(id: 100) {
        FrameLayout()
            .width(.matchParent)
            .height(.matchParent)
    }
    .width(.matchParent)
    .height(.matchParent)
}
```

You can also create it without child content:

```swift
let drawerLayout = DrawerLayout(id: 100)
```

## Current limitations

Droid does not currently expose DrawerLayout-specific open, close, lock-mode, state-listener, or drawer-gravity helpers on this type.

Use the wrapper as the AndroidX drawer container and rely only on inherited `ViewGroup` APIs that are exposed by Droid.
